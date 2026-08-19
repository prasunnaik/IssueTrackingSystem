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


    // -------------------------------------------------------
    // Check logged-in user
    // -------------------------------------------------------

    if (!storedUser) {

      this.router.navigate([
        '/login'
      ]);

      return;
    }


    // -------------------------------------------------------
    // Read user from localStorage
    // -------------------------------------------------------

    try {

      const user =
        JSON.parse(storedUser);


      this.assigneeId =
        Number(user.id);


      this.assigneeName =
        user.name || 'Assignee';


      this.assigneeEmail =
        user.email || '';


      /*
       * Your User model uses:
       *
       * profileImage
       *
       * So we read that value here.
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


    // -------------------------------------------------------
    // Validate ID
    // -------------------------------------------------------

    if (!this.assigneeId) {

      this.errorMessage =
        'Logged-in user information is missing.';

      return;
    }


    // -------------------------------------------------------
    // Load assigned issues
    // -------------------------------------------------------

    this.loadIssues();
  }


  // =========================================================
  // LOAD ASSIGNED ISSUES
  // =========================================================

  loadIssues(): void {

    this.loading = true;

    this.errorMessage = '';

    this.successMessage = '';


    this.issueService
      .getIssuesByAssignee(this.assigneeId)
      .subscribe({

        next: (data: Issue[]) => {

          this.issues =
            data || [];

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
  // GO TO DASHBOARD
  // =========================================================

  goToDashboard(): void {

    /*
     * We are already on the dashboard.
     *
     * Scroll back to the top instead of
     * unnecessarily reloading the route.
     */

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


    return this.issues.filter(
      issue =>

        issue.summary
          ?.toLowerCase()
          .includes(search)

        ||

        issue.description
          ?.toLowerCase()
          .includes(search)
    );
  }


  // =========================================================
  // TO DO ISSUES
  // =========================================================

  getTodoIssues(): Issue[] {

    return this.filteredIssues.filter(
      issue =>
        issue.status === 'OPEN'
    );
  }


  // =========================================================
  // DEVELOPMENT ISSUES
  // =========================================================

  getDevelopmentIssues(): Issue[] {

    return this.filteredIssues.filter(
      issue =>
        issue.status === 'IN_PROGRESS'
    );
  }


  // =========================================================
  // TESTING ISSUES
  // =========================================================

  getTestingIssues(): Issue[] {

    return this.filteredIssues.filter(
      issue =>
        issue.status === 'TESTING'
    );
  }


  // =========================================================
  // COMPLETED ISSUES
  // =========================================================

  getCompletedIssues(): Issue[] {

    return this.filteredIssues.filter(
      issue =>
        issue.status === 'CLOSED'
    );
  }


  // =========================================================
  // TOTAL COUNT
  // =========================================================

  getTotalIssues(): number {

    return this.issues.length;
  }


  // =========================================================
  // TO DO COUNT
  // =========================================================

  getTodoCount(): number {

    return this.issues.filter(
      issue =>
        issue.status === 'OPEN'
    ).length;
  }


  // =========================================================
  // DEVELOPMENT COUNT
  // =========================================================

  getDevelopmentCount(): number {

    return this.issues.filter(
      issue =>
        issue.status === 'IN_PROGRESS'
    ).length;
  }


  // =========================================================
  // TESTING COUNT
  // =========================================================

  getTestingCount(): number {

    return this.issues.filter(
      issue =>
        issue.status === 'TESTING'
    ).length;
  }


  // =========================================================
  // COMPLETED COUNT
  // =========================================================

  getCompletedCount(): number {

    return this.issues.filter(
      issue =>
        issue.status === 'CLOSED'
    ).length;
  }


  // =========================================================
  // OPEN ISSUE DETAILS
  // =========================================================

  openIssue(issue: Issue): void {

    if (
      issue.id === undefined ||
      issue.id === null
    ) {

      alert(
        'Issue ID is missing.'
      );

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

  getStatusClass(
    status: string
  ): string {

    if (
      status === 'OPEN'
    ) {

      return 'todo';
    }


    if (
      status === 'IN_PROGRESS'
    ) {

      return 'development';
    }


    if (
      status === 'TESTING'
    ) {

      return 'testing';
    }


    if (
      status === 'CLOSED'
    ) {

      return 'completed';
    }


    return '';
  }


  // =========================================================
  // PRIORITY CLASS
  // =========================================================

  getPriorityClass(
    priority: string
  ): string {

    if (
      priority === 'HIGH'
    ) {

      return 'high';
    }


    if (
      priority === 'MEDIUM'
    ) {

      return 'medium';
    }


    if (
      priority === 'LOW'
    ) {

      return 'low';
    }


    return '';
  }


  // =========================================================
  // LOGOUT
  // =========================================================

  logout(): void {

    localStorage.removeItem(
      'user'
    );


    this.router.navigate([
      '/login'
    ]);
  }

}
