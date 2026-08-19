import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { Router } from '@angular/router';

import { IssueService } from '../../services/issue.service';
import { Issue } from '../../models/issue';

@Component({
  selector: 'app-assignee-dashboard',
  standalone: true,
  imports: [
    CommonModule,
    FormsModule
  ],
  templateUrl: './assignee-dashboard.component.html',
  styleUrl: './assignee-dashboard.component.css'
})
export class AssigneeDashboardComponent implements OnInit {

  // =========================================================
  // ASSIGNEE
  // =========================================================

  assigneeId: number = 0;

  assigneeName: string = 'Assignee';

  assigneeEmail: string = '';

  profileImage: string = '';


  // =========================================================
  // ISSUES
  // =========================================================

  issues: Issue[] = [];

  loading: boolean = false;

  errorMessage: string = '';

  successMessage: string = '';


  // =========================================================
  // SEARCH
  // =========================================================

  searchText: string = '';


  // =========================================================
  // CONSTRUCTOR
  // =========================================================

  constructor(
    private issueService: IssueService,
    private router: Router
  ) {}


  // =========================================================
  // INIT
  // =========================================================

  ngOnInit(): void {

    const storedUser =
      localStorage.getItem('user');

    if (!storedUser) {

      this.router.navigate(['/login']);

      return;
    }

    try {

      const user = JSON.parse(storedUser);

      this.assigneeId = Number(user.id);

      this.assigneeName =
        user.name || 'Assignee';

      this.assigneeEmail =
        user.email || '';

      /*
       * IMPORTANT:
       * profileImage comes from the backend User object.
       *
       * Example:
       * user.profileImage =
       * "https://....jpg"
       */

      this.profileImage =
        user.profileImage || '';

    } catch (error) {

      console.error(
        'Failed to read logged-in user:',
        error
      );

      this.errorMessage =
        'Invalid logged-in user information.';

      return;
    }

    if (!this.assigneeId) {

      this.errorMessage =
        'Logged-in user information is missing.';

      return;
    }

    this.loadIssues();
  }


  // =========================================================
  // PROFILE IMAGE ERROR
  // =========================================================

  onProfileImageError(): void {

    /*
     * If the image URL is invalid,
     * remove it so Angular displays
     * the fallback initial.
     */

    this.profileImage = '';
  }


  // =========================================================
  // LOAD ISSUES
  // =========================================================

  loadIssues(): void {

    this.loading = true;

    this.errorMessage = '';

    this.successMessage = '';

    this.issueService
      .getIssuesByAssignee(this.assigneeId)
      .subscribe({

        next: (data: Issue[]) => {

          this.issues = data || [];

          this.loading = false;
        },

        error: (error: any) => {

          console.error(
            'Failed to load assigned issues:',
            error
          );

          this.issues = [];

          this.loading = false;

          this.errorMessage =
            'Failed to load assigned issues.';
        }
      });
  }


  // =========================================================
  // REFRESH
  // =========================================================

  refreshDashboard(): void {

    this.successMessage = '';

    this.loadIssues();
  }


  // =========================================================
  // DASHBOARD
  // =========================================================

  goToDashboard(): void {

    window.scrollTo({
      top: 0,
      behavior: 'smooth'
    });
  }


  // =========================================================
  // SEARCH
  // =========================================================

  get filteredIssues(): Issue[] {

    const search =
      this.searchText
        .trim()
        .toLowerCase();

    if (!search) {

      return this.issues;
    }

    return this.issues.filter(issue => {

      const summary =
        issue.summary?.toLowerCase() || '';

      const description =
        issue.description?.toLowerCase() || '';

      return (
        summary.includes(search) ||
        description.includes(search)
      );
    });
  }


  // =========================================================
  // TODO
  // =========================================================

  getTodoIssues(): Issue[] {

    return this.filteredIssues.filter(
      issue => issue.status === 'OPEN'
    );
  }


  // =========================================================
  // DEVELOPMENT
  // =========================================================

  getDevelopmentIssues(): Issue[] {

    return this.filteredIssues.filter(
      issue => issue.status === 'IN_PROGRESS'
    );
  }


  // =========================================================
  // TESTING
  // =========================================================

  getTestingIssues(): Issue[] {

    return this.filteredIssues.filter(
      issue => issue.status === 'TESTING'
    );
  }


  // =========================================================
  // COMPLETED
  // =========================================================

  getCompletedIssues(): Issue[] {

    return this.filteredIssues.filter(
      issue => issue.status === 'CLOSED'
    );
  }


  // =========================================================
  // COUNTS
  // =========================================================

  getTotalIssues(): number {

    return this.issues.length;
  }


  getTodoCount(): number {

    return this.issues.filter(
      issue => issue.status === 'OPEN'
    ).length;
  }


  getDevelopmentCount(): number {

    return this.issues.filter(
      issue => issue.status === 'IN_PROGRESS'
    ).length;
  }


  getTestingCount(): number {

    return this.issues.filter(
      issue => issue.status === 'TESTING'
    ).length;
  }


  getCompletedCount(): number {

    return this.issues.filter(
      issue => issue.status === 'CLOSED'
    ).length;
  }


  // =========================================================
  // OPEN ISSUE
  // =========================================================

  openIssue(issue: Issue): void {

    if (
      issue.id === undefined ||
      issue.id === null
    ) {

      alert('Issue ID is missing.');

      return;
    }

    this.router.navigate([
      '/assignee-issue',
      issue.id
    ]);
  }


  // =========================================================
  // STATUS CLASS
  // =========================================================

  getStatusClass(status: string): string {

    switch (status) {

      case 'OPEN':
        return 'todo';

      case 'IN_PROGRESS':
        return 'development';

      case 'TESTING':
        return 'testing';

      case 'CLOSED':
        return 'completed';

      default:
        return '';
    }
  }


  // =========================================================
  // PRIORITY CLASS
  // =========================================================

  getPriorityClass(priority: string): string {

    switch (priority) {

      case 'HIGH':
        return 'high';

      case 'MEDIUM':
        return 'medium';

      case 'LOW':
        return 'low';

      default:
        return '';
    }
  }


  // =========================================================
  // LOGOUT
  // =========================================================

  logout(): void {

    localStorage.removeItem('user');

    this.router.navigate(['/login']);
  }

}
