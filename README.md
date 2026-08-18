import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';

import { IssueService } from '../../services/issue.service';
import { Issue } from '../../models/issue';
import { Project } from '../../models/project';

@Component({
  selector: 'app-owner-dashboard',
  standalone: true,
  imports: [
    CommonModule,
    FormsModule
  ],
  templateUrl: './owner-dashboard.component.html',
  styleUrls: ['./owner-dashboard.component.css']
})
export class OwnerDashboardComponent implements OnInit {

  // =========================================================
  // PROJECT
  // =========================================================

  project: Project = {
    id: 3,
    projectName: 'PMS',
    projectOwnerId: 1,
    startDate: '2026-08-10',
    endDate: '2027-01-31'
  };

  // =========================================================
  // ISSUES
  // =========================================================

  issues: Issue[] = [];

  // =========================================================
  // FILTER
  // =========================================================

  selectedFilter: string = 'ALL';

  // =========================================================
  // ERROR
  // =========================================================

  errorMessage: string = '';

  // =========================================================
  // CONSTRUCTOR
  // =========================================================

  constructor(
    private issueService: IssueService
  ) {}

  // =========================================================
  // INIT
  // =========================================================

  ngOnInit(): void {
    this.loadIssues();
  }

  // =========================================================
  // LOAD ISSUES
  // =========================================================

  loadIssues(): void {

    if (this.project.id === undefined || this.project.id === null) {
      this.errorMessage = 'Project ID is missing.';
      return;
    }

    this.issueService
      .getIssuesByProject(this.project.id)
      .subscribe({

        next: (data: Issue[]) => {

          this.issues = data || [];

          this.errorMessage = '';

          console.log('Issues loaded:', this.issues);
        },

        error: (error) => {

          console.error(
            'Failed to load issues:',
            error
          );

          this.errorMessage = 'Failed to load issues.';
        }

      });
  }

  // =========================================================
  // REFRESH DASHBOARD
  // =========================================================

  refreshDashboard(): void {
    this.loadIssues();
  }

  // =========================================================
  // FILTER
  // =========================================================

  filterIssues(filter: string): void {
    this.selectedFilter = filter;
  }

  get filteredIssues(): Issue[] {

    if (this.selectedFilter === 'ALL') {
      return this.issues;
    }

    if (this.selectedFilter === 'OPEN') {
      return this.issues.filter(
        issue => issue.status === 'OPEN'
      );
    }

    if (this.selectedFilter === 'IN_PROGRESS') {
      return this.issues.filter(
        issue => issue.status === 'IN_PROGRESS'
      );
    }

    if (this.selectedFilter === 'HIGH') {
      return this.issues.filter(
        issue => issue.priority === 'HIGH'
      );
    }

    if (this.selectedFilter === 'MEDIUM') {
      return this.issues.filter(
        issue => issue.priority === 'MEDIUM'
      );
    }

    if (this.selectedFilter === 'CLOSED') {
      return this.issues.filter(
        issue => issue.status === 'CLOSED'
      );
    }

    return this.issues;
  }

  // =========================================================
  // DASHBOARD COUNTS
  // =========================================================

  getTotalIssues(): number {
    return this.issues.length;
  }

  getOpenIssues(): number {

    return this.issues.filter(
      issue => issue.status === 'OPEN'
    ).length;

  }

  getHighPriorityIssues(): number {

    return this.issues.filter(
      issue => issue.priority === 'HIGH'
    ).length;

  }

  getInProgressIssues(): number {

    return this.issues.filter(
      issue => issue.status === 'IN_PROGRESS'
    ).length;

  }

  getClosedIssues(): number {

    return this.issues.filter(
      issue => issue.status === 'CLOSED'
    ).length;

  }

  // =========================================================
  // UPDATE STATUS
  // =========================================================

  updateStatus(
    issue: Issue,
    event: Event
  ): void {

    if (
      issue.id === undefined ||
      issue.id === null
    ) {
      alert('Issue ID is missing.');
      return;
    }

    const select =
      event.target as HTMLSelectElement;

    const status = select.value;

    if (!status) {
      alert('Please select a valid status.');
      return;
    }

    console.log(
      'Updating status:',
      issue.id,
      status
    );

    this.issueService
      .updateIssueStatus(
        issue.id,
        status
      )
      .subscribe({

        next: (updatedIssue: Issue) => {

          issue.status =
            updatedIssue.status;

        },

        error: (error) => {

          console.error(
            'Failed to update issue status:',
            error
          );

          alert(
            'Failed to update issue status.'
          );

        }

      });
  }

  // =========================================================
  // UPDATE PRIORITY
  // =========================================================

  updatePriority(
    issue: Issue,
    event: Event
  ): void {

    if (
      issue.id === undefined ||
      issue.id === null
    ) {
      alert('Issue ID is missing.');
      return;
    }

    const select =
      event.target as HTMLSelectElement;

    const priority = select.value;

    if (!priority) {
      alert('Please select a valid priority.');
      return;
    }

    console.log(
      'Updating priority:',
      issue.id,
      priority
    );

    this.issueService
      .updateIssuePriority(
        issue.id,
        priority
      )
      .subscribe({

        next: (updatedIssue: Issue) => {

          issue.priority =
            updatedIssue.priority;

        },

        error: (error) => {

          console.error(
            'Failed to update issue priority:',
            error
          );

          alert(
            'Failed to update issue priority.'
          );

        }

      });
  }

  // =========================================================
  // UPDATE ASSIGNEE
  // =========================================================

  updateAssignee(
    issue: Issue,
    event: Event
  ): void {

    // Check issue ID
    if (
      issue.id === undefined ||
      issue.id === null
    ) {
      alert('Issue ID is missing.');
      return;
    }

    const select =
      event.target as HTMLSelectElement;

    /*
     * Angular [ngValue] can produce an internal
     * select value such as:
     *
     * 0: 1
     * 1: 2
     * 2: 3
     * 3: 4
     *
     * Therefore Number(select.value) directly
     * can produce NaN.
     */

    const rawValue = select.value;

    console.log(
      'Raw assignee value:',
      rawValue
    );

    let assigneeId: number;

    // Handle Angular [ngValue]
    if (rawValue.includes(':')) {

      const parts =
        rawValue.split(':');

      const lastPart =
        parts[parts.length - 1].trim();

      assigneeId =
        Number(lastPart);

    } else {

      // Handle normal value="4"
      assigneeId =
        Number(rawValue);

    }

    console.log(
      'Parsed assignee ID:',
      assigneeId
    );

    // Validate
    if (
      !Number.isInteger(assigneeId) ||
      assigneeId <= 0
    ) {

      alert(
        'Please select a valid assignee.'
      );

      return;
    }

    console.log(
      'Updating assignee:',
      issue.id,
      assigneeId
    );

    // Call backend
    this.issueService
      .updateIssueAssignee(
        issue.id,
        assigneeId
      )
      .subscribe({

        next: (updatedIssue: Issue) => {

          console.log(
            'Assignee updated successfully:',
            updatedIssue
          );

          issue.assigneeId =
            updatedIssue.assigneeId;

        },

        error: (error) => {

          console.error(
            'Failed to update issue assignee:',
            error
          );

          alert(
            'Failed to update issue assignee.'
          );

        }

      });
  }

  // =========================================================
  // STATUS CSS CLASS
  // =========================================================

  getStatusClass(
    status: string
  ): string {

    if (status === 'OPEN') {
      return 'open';
    }

    if (status === 'IN_PROGRESS') {
      return 'progress';
    }

    if (status === 'CLOSED') {
      return 'closed';
    }

    return '';
  }

  // =========================================================
  // PRIORITY CSS CLASS
  // =========================================================

  getPriorityClass(
    priority: string
  ): string {

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

}
