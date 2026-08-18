import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';

import { Project } from '../../models/project';
import { Issue } from '../../models/issue';

import { ProjectService } from '../../services/project.service';
import { IssueService } from '../../services/issue.service';

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

  project: Project | null = null;

  issues: Issue[] = [];

  filteredIssues: Issue[] = [];

  selectedFilter: string = 'ALL';

  errorMessage: string = '';

  // Change this if your owner/project ID is different
  ownerId: number = 1;

  constructor(
    private projectService: ProjectService,
    private issueService: IssueService
  ) {}

  ngOnInit(): void {
    this.loadDashboard();
  }

  loadDashboard(): void {

    this.errorMessage = '';

    /*
     * If your existing project-loading code already works,
     * keep that code here.
     *
     * The dashboard currently uses Project ID 3.
     */
    this.loadProject(3);
  }

  loadProject(projectId: number): void {

    this.projectService.getProjectById(projectId)
      .subscribe({
        next: (response: Project) => {

          this.project = response;

          this.loadIssues(projectId);
        },

        error: (error) => {

          console.error(
            'Failed to load project:',
            error
          );

          this.errorMessage =
            'Failed to load project.';
        }
      });
  }

  loadIssues(projectId: number): void {

    this.issueService.getIssuesByProject(projectId)
      .subscribe({
        next: (response: Issue[]) => {

          this.issues = response;

          this.filteredIssues = response;
        },

        error: (error) => {

          console.error(
            'Failed to load issues:',
            error
          );

          this.errorMessage =
            'Failed to load issues.';
        }
      });
  }

  refresh(): void {

    if (this.project) {

      this.loadIssues(
        this.project.id
      );

    } else {

      this.loadProject(3);
    }
  }

  // ==========================================
  // FILTERS
  // ==========================================

  filterIssues(filter: string): void {

    this.selectedFilter = filter;

    switch (filter) {

      case 'OPEN':

        this.filteredIssues = this.issues.filter(
          issue => issue.status === 'OPEN'
        );

        break;

      case 'IN_PROGRESS':

        this.filteredIssues = this.issues.filter(
          issue => issue.status === 'IN_PROGRESS'
        );

        break;

      case 'HIGH':

        this.filteredIssues = this.issues.filter(
          issue => issue.priority === 'HIGH'
        );

        break;

      case 'MEDIUM':

        this.filteredIssues = this.issues.filter(
          issue => issue.priority === 'MEDIUM'
        );

        break;

      case 'CLOSED':

        this.filteredIssues = this.issues.filter(
          issue => issue.status === 'CLOSED'
        );

        break;

      case 'ALL':

      default:

        this.filteredIssues = this.issues;

        break;
    }
  }

  // ==========================================
  // STATISTICS
  // ==========================================

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

  // ==========================================
  // STATUS UPDATE
  // ==========================================

  updateStatus(
    issue: Issue,
    status: string
  ): void {

    this.issueService
      .updateIssueStatus(issue.id, status)
      .subscribe({

        next: (response: Issue) => {

          issue.status = response.status;

          this.filterIssues(
            this.selectedFilter
          );
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

  // ==========================================
  // PRIORITY UPDATE
  // ==========================================

  updatePriority(
    issue: Issue,
    priority: string
  ): void {

    this.issueService
      .updateIssuePriority(
        issue.id,
        priority
      )
      .subscribe({

        next: (response: Issue) => {

          issue.priority = response.priority;

          this.filterIssues(
            this.selectedFilter
          );
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

  // ==========================================
  // ASSIGNEE UPDATE
  // ==========================================

  updateAssignee(
    issue: Issue,
    assigneeId: number
  ): void {

    this.issueService
      .updateIssueAssignee(
        issue.id,
        assigneeId
      )
      .subscribe({

        next: (response: Issue) => {

          issue.assigneeId =
            response.assigneeId;
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
}
