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

  errorMessage: string = '';

  constructor(
    private projectService: ProjectService,
    private issueService: IssueService
  ) {}

  ngOnInit(): void {
    this.loadProjects();
  }


  // =========================
  // LOAD PROJECTS
  // =========================

  loadProjects(): void {

    this.errorMessage = '';

    this.projectService.getProjectsByOwner(1).subscribe({

      next: (response: any[]) => {

        console.log('Projects received:', response);

        this.projects = response;

        if (this.projects.length > 0) {

          const projectId = this.projects[0].id;

          this.loadIssues(projectId);

        } else {

          this.issues = [];

        }

      },

      error: (error: any) => {

        console.error('Error loading projects:', error);

        this.errorMessage = 'Unable to load projects.';

        this.projects = [];
        this.issues = [];

      }

    });
  }


  // =========================
  // LOAD ISSUES
  // =========================

  loadIssues(projectId: number): void {

    this.issueService.getIssuesByProject(projectId).subscribe({

      next: (response: any[]) => {

        console.log('Issues received:', response);

        this.issues = response;

      },

      error: (error: any) => {

        console.error('Error loading issues:', error);

        this.errorMessage = 'Unable to load issues.';

        this.issues = [];

      }

    });

  }


  // =========================
  // COUNT OPEN ISSUES
  // =========================

  getOpenIssues(): number {

    return this.issues.filter(
      issue => issue.status === 'OPEN'
    ).length;

  }


  // =========================
  // COUNT HIGH PRIORITY
  // =========================

  getHighPriorityIssues(): number {

    return this.issues.filter(
      issue => issue.priority === 'HIGH'
    ).length;

  }


  // =========================
  // UPDATE ISSUE STATUS
  // =========================

  updateStatus(issue: any, status: string): void {

    const updatedIssue = {
      ...issue,
      status: status
    };

    console.log(
      'Updating issue:',
      issue.id,
      updatedIssue
    );

    this.issueService
      .updateIssue(issue.id, updatedIssue)
      .subscribe({

        next: (response: any) => {

          console.log(
            'Issue updated successfully:',
            response
          );

          // Update the issue on screen
          issue.status = status;

        },

        error: (error: any) => {

          console.error(
            'Error updating issue:',
            error
          );

          this.errorMessage =
            'Unable to update issue status.';

        }

      });

  }

}
