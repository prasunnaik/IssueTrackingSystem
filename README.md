import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';

import { ProjectService } from '../../services/project.service';
import { IssueService } from '../../services/issue.service';

@Component({
  selector: 'app-owner-dashboard',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './owner-dashboard.component.html',
  styleUrls: ['./owner-dashboard.component.css']
})
export class OwnerDashboardComponent implements OnInit {

  projects: any[] = [];
  issues: any[] = [];

  errorMessage = '';

  constructor(
    private projectService: ProjectService,
    private issueService: IssueService
  ) {}

  ngOnInit(): void {
    this.loadProjects();
  }

  loadProjects(): void {

    this.projectService.getProjectsByOwner(1).subscribe({

      next: (response: any[]) => {

        this.projects = response;

        if (this.projects.length > 0) {
          this.loadIssues(this.projects[0].id);
        }

      },

      error: (error) => {
        console.error(error);
        this.errorMessage = 'Unable to load projects.';
      }

    });
  }

  loadIssues(projectId: number): void {

    this.issueService.getIssuesByProject(projectId).subscribe({

      next: (response: any[]) => {
        this.issues = response;
      },

      error: (error) => {
        console.error(error);
        this.errorMessage = 'Unable to load issues.';
      }

    });
  }

  updateStatus(issue: any, status: string): void {

    const updatedIssue = {
      ...issue,
      status: status
    };

    this.issueService.updateIssue(issue.id, updatedIssue).subscribe({

      next: (response) => {

        issue.status = status;

        console.log('Issue updated successfully', response);

      },

      error: (error) => {

        console.error('Update failed', error);

        this.errorMessage = 'Unable to update issue status.';

      }

    });
  }

}
