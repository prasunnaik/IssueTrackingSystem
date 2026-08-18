import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { ActivatedRoute, Router } from '@angular/router';

import { IssueService } from '../../services/issue.service';
import { Issue } from '../../models/issue';

@Component({
  selector: 'app-edit-issue',
  standalone: true,
  imports: [
    CommonModule,
    FormsModule
  ],
  templateUrl: './edit-issue.component.html',
  styleUrl: './edit-issue.component.css'
})
export class EditIssueComponent implements OnInit {

  issue: Issue = {
    summary: '',
    description: '',
    status: 'OPEN',
    priority: 'MEDIUM',
    type: 'BUG',
    storyPoint: 1,
    sprint: '',
    tags: '',
    projectId: undefined,
    assigneeId: undefined
  };

  issueId: number = 0;

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

    const id = Number(
      this.route.snapshot.paramMap.get('id')
    );

    if (!id) {

      this.errorMessage =
        'Invalid issue ID.';

      this.loading = false;

      return;
    }

    this.issueId = id;

    this.loadIssue();
  }

  loadIssue(): void {

    this.issueService
      .getIssueById(this.issueId)
      .subscribe({

        next: (data: Issue) => {

          this.issue = {
            ...data
          };

          this.loading = false;
        },

        error: (error) => {

          console.error(
            'Failed to load issue:',
            error
          );

          this.errorMessage =
            'Failed to load issue.';

          this.loading = false;
        }
      });
  }

  isFormValid(): boolean {

    if (!this.issue.summary.trim()) {
      return false;
    }

    if (
      this.issue.summary.length < 5 ||
      this.issue.summary.length > 150
    ) {
      return false;
    }

    if (!this.issue.description.trim()) {
      return false;
    }

    if (this.issue.description.length > 500) {
      return false;
    }

    if (!this.issue.type) {
      return false;
    }

    if (!this.issue.priority) {
      return false;
    }

    if (!this.issue.status) {
      return false;
    }

    if (
      !this.issue.assigneeId ||
      this.issue.assigneeId <= 0
    ) {
      return false;
    }

    if (
      !this.issue.storyPoint ||
      this.issue.storyPoint <= 0
    ) {
      return false;
    }

    return true;
  }

  saveIssue(): void {

    this.errorMessage = '';
    this.successMessage = '';

    if (!this.isFormValid()) {

      this.errorMessage =
        'Please fill all required fields correctly.';

      return;
    }

    this.saving = true;

    this.issueService
      .updateIssue(
        this.issueId,
        this.issue
      )
      .subscribe({

        next: (updatedIssue: Issue) => {

          this.saving = false;

          this.successMessage =
            'Issue updated successfully.';

          setTimeout(() => {

            this.router.navigate([
              '/issue',
              this.issueId
            ]);

          }, 700);
        },

        error: (error) => {

          console.error(
            'Failed to update issue:',
            error
          );

          this.saving = false;

          this.errorMessage =
            'Failed to update issue.';
        }
      });
  }

  resetForm(): void {

    this.loadIssue();
    this.errorMessage = '';
    this.successMessage = '';
  }

  cancel(): void {

    this.router.navigate([
      '/issue',
      this.issueId
    ]);
  }
}
