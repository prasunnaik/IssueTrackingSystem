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

  // Temporary hardcoded ID.
  // Later this will come from login/authentication.
  assigneeId: number = 1;

  assigneeName: string = 'Assignee';


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
    this.loadIssues();
  }


  // =========================================================
  // LOAD ASSIGNED ISSUES
  // =========================================================

  loadIssues(): void {

    this.loading = true;
    this.errorMessage = '';

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

    return this.issues.filter(issue =>

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
  // TODO ISSUES
  // =========================================================

  getTodoIssues(): Issue[] {

    return this.filteredIssues.filter(
      issue => issue.status === 'OPEN'
    );
  }


  // =========================================================
  // DEVELOPMENT ISSUES
  // =========================================================

  getDevelopmentIssues(): Issue[] {

    return this.filteredIssues.filter(
      issue => issue.status === 'IN_PROGRESS'
    );
  }


  // =========================================================
  // TESTING ISSUES
  // =========================================================

  getTestingIssues(): Issue[] {

    return this.filteredIssues.filter(
      issue => issue.status === 'TESTING'
    );
  }


  // =========================================================
  // COMPLETED ISSUES
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
  // OPEN ISSUE DETAILS
  // =========================================================

  openIssue(issue: Issue): void {

    if (issue.id === undefined) {

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

    if (status === 'OPEN') {
      return 'todo';
    }

    if (status === 'IN_PROGRESS') {
      return 'development';
    }

    if (status === 'TESTING') {
      return 'testing';
    }

    if (status === 'CLOSED') {
      return 'completed';
    }

    return '';
  }


  // =========================================================
  // PRIORITY CLASS
  // =========================================================

  getPriorityClass(priority: string): string {

    if (priority === 'HIGH') {
      return 'high';
    }

    if (priority === 'MEDIUM') {
      return 'medium';
    }

    if (priority === 'LOW') {
      return 'low';
    }

    return '';
  }


  // =========================================================
  // LOGOUT
  // =========================================================

  logout(): void {

    this.router.navigate([
      '/login'
    ]);
  }
}
