import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ActivatedRoute, Router } from '@angular/router';

import { IssueService } from '../../services/issue.service';
import { Issue } from '../../models/issue';

@Component({
  selector: 'app-issue-details',
  standalone: true,
  imports: [
    CommonModule
  ],
  templateUrl: './issue-details.component.html',
  styleUrl: './issue-details.component.css'
})
export class IssueDetailsComponent implements OnInit {

  issue: Issue | null = null;

  loading: boolean = true;
  errorMessage: string = '';

  constructor(
    private route: ActivatedRoute,
    private router: Router,
    private issueService: IssueService
  ) {}

  ngOnInit(): void {

    const id = Number(
      this.route.snapshot.paramMap.get('id')
    );

    if (!id) {
      this.errorMessage = 'Invalid issue ID.';
      this.loading = false;
      return;
    }

    this.loadIssue(id);
  }

  loadIssue(issueId: number): void {

    this.loading = true;
    this.errorMessage = '';

    this.issueService
      .getIssueById(issueId)
      .subscribe({

        next: (data: Issue) => {

          this.issue = data;
          this.loading = false;
        },

        error: (error) => {

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

  editIssue(): void {

    if (!this.issue?.id) {
      return;
    }

    this.router.navigate([
      '/edit-issue',
      this.issue.id
    ]);
  }

  deleteIssue(): void {

    if (!this.issue?.id) {
      return;
    }

    const confirmed = confirm(
      'Are you sure you want to delete this issue?'
    );

    if (!confirmed) {
      return;
    }

    this.issueService
      .deleteIssue(this.issue.id)
      .subscribe({

        next: () => {

          alert('Issue deleted successfully.');

          this.router.navigate([
            '/owner-dashboard'
          ]);
        },

        error: (error) => {

          console.error(
            'Failed to delete issue:',
            error
          );

          alert(
            'Failed to delete issue.'
          );
        }
      });
  }

  goBack(): void {

    this.router.navigate([
      '/owner-dashboard'
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
