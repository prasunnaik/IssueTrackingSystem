import { Component, OnInit } from '@angular/core';
import { IssueService } from '../../services/issue.service';
import { Issue } from '../../models/issue';
import { Project } from '../../models/project';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';

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

  // Project currently displayed
  project: Project = {
    id: 3,
    projectName: 'PMS',
    projectOwnerId: 1,
    startDate: '2026-08-10',
    endDate: '2027-01-31'
  };

  // Issues belonging to the project
  issues: Issue[] = [];

  // Filter
  selectedFilter: string = 'ALL';

  // Error message
  errorMessage: string = '';

  constructor(
    private issueService: IssueService
  ) {}

  ngOnInit(): void {
    this.loadIssues();
  }

  // =========================================================
  // LOAD ISSUES
  // =========================================================

  loadIssues(): void {

    if (this.project.id === undefined) {
      this.errorMessage = 'Project ID is missing.';
      return;
    }

    this.issueService
      .getIssuesByProject(this.project.id)
      .subscribe({
        next: (data: Issue[]) => {
          this.issues = data || [];
          this.errorMessage = '';
        },

        error: (error) => {
          console.error('Failed to load issues:', error);
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

  updateStatus(issue: Issue, event: Event): void {

    if (issue.id === undefined) {
      alert('Issue ID is missing.');
      return;
    }

    const select = event.target as HTMLSelectElement;
    const status = select.value;

    this.issueService
      .updateIssueStatus(issue.id, status)
      .subscribe({

        next: (updatedIssue: Issue) => {
          issue.status = updatedIssue.status;
        },

        error: (error) => {
          console.error(
            'Failed to update issue status:',
            error
          );

          alert('Failed to update issue status.');
        }
      });
  }

  // =========================================================
  // UPDATE PRIORITY
  // =========================================================

  updatePriority(issue: Issue, event: Event): void {

    if (issue.id === undefined) {
      alert('Issue ID is missing.');
      return;
    }

    const select = event.target as HTMLSelectElement;
    const priority = select.value;

    this.issueService
      .updateIssuePriority(issue.id, priority)
      .subscribe({

        next: (updatedIssue: Issue) => {
          issue.priority = updatedIssue.priority;
        },

        error: (error) => {
          console.error(
            'Failed to update issue priority:',
            error
          );

          alert('Failed to update issue priority.');
        }
      });
  }

  // =========================================================
  // UPDATE ASSIGNEE
  // =========================================================

  updateAssignee(issue: Issue, event: Event): void {

    if (issue.id === undefined) {
      alert('Issue ID is missing.');
      return;
    }

    const select = event.target as HTMLSelectElement;

    const assigneeId = Number(select.value);

    if (!assigneeId) {
      alert('Please select a valid assignee.');
      return;
    }

    this.issueService
      .updateIssueAssignee(issue.id, assigneeId)
      .subscribe({

        next: (updatedIssue: Issue) => {
          issue.assigneeId = updatedIssue.assigneeId;
        },

        error: (error) => {
          console.error(
            'Failed to update issue assignee:',
            error
          );

          alert('Failed to update issue assignee.');
        }
      });
  }

  // =========================================================
  // STATUS CLASS
  // =========================================================

  getStatusClass(status: string): string {

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
}
