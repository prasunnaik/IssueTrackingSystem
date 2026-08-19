import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { ActivatedRoute, Router } from '@angular/router';

import { IssueService } from '../../services/issue.service';
import { Issue } from '../../models/issue';

@Component({
  selector: 'app-assignee-issue-details',
  standalone: true,
  imports: [
    CommonModule,
    FormsModule
  ],
  templateUrl: './assignee-issue-details.component.html',
  styleUrl: './assignee-issue-details.component.css'
})
export class AssigneeIssueDetailsComponent implements OnInit {

  issue: Issue | null = null;

  selectedStatus: string = '';

  originalStatus: string = '';

  loading: boolean = true;

  saving: boolean = false;

  errorMessage: string = '';

  successMessage: string = '';

  constructor(
    private route: ActivatedRoute,
    private router: Router,
    private issueService: IssueService
  ) {}

  ngOnInit(): void {

    const issueId = Number(
      this.route.snapshot.paramMap.get('id')
    );

    if (!issueId) {
      this.errorMessage = 'Invalid issue ID.';
      this.loading = false;
      return;
    }

    this.loadIssue(issueId);
  }

  loadIssue(issueId: number): void {

    this.loading = true;
    this.errorMessage = '';

    this.issueService
      .getIssueById(issueId)
      .subscribe({

        next: (data: Issue) => {

          this.issue = data;

          this.selectedStatus = data.status;

          this.originalStatus = data.status;

          this.loading = false;
        },

        error: (error: any) => {

          console.error(
            'Failed to load issue:',
            error
          );

          this.errorMessage =
            'Failed to load issue details.';

          this.loading = false;
        }
      });
  }

  onStatusChange(): void {

    this.successMessage = '';
    this.errorMessage = '';
  }

  hasStatusChanged(): boolean {

    return this.selectedStatus !== this.originalStatus;
  }

  saveUpdates(): void {

    if (!this.issue || this.issue.id === undefined) {
      this.errorMessage = 'Issue information is missing.';
      return;
    }

    if (!this.hasStatusChanged()) {
      return;
    }

    this.saving = true;

    this.errorMessage = '';
    this.successMessage = '';

    this.issueService
      .updateIssueStatus(
        this.issue.id,
        this.selectedStatus
      )
      .subscribe({

        next: (updatedIssue: Issue) => {

          this.issue = updatedIssue;

          this.selectedStatus =
            updatedIssue.status;

          this.originalStatus =
            updatedIssue.status;

          this.saving = false;

          this.successMessage =
            'Issue status updated successfully.';
        },

        error: (error: any) => {

          console.error(
            'Failed to update issue status:',
            error
          );

          this.saving = false;

          this.errorMessage =
            'Failed to update issue status.';

          this.selectedStatus =
            this.originalStatus;
        }
      });
  }

  goBack(): void {

    this.router.navigate([
      '/assignee-dashboard'
    ]);
  }

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
