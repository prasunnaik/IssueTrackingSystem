import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';

import { IssueService } from '../../services/issue.service';
import { ProjectService } from '../../services/project.service';

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
  // OWNER
  // =========================================================

  ownerId: number = 1;

  // =========================================================
  // PROJECTS
  // =========================================================

  projects: Project[] = [];

  selectedProject: Project | null = null;

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

  constructor(
    private projectService: ProjectService,
    private issueService: IssueService
  ) {}

  // =========================================================
  // INITIALIZE
  // =========================================================

  ngOnInit(): void {

    this.loadProjects();
  }

  // =========================================================
  // LOAD OWNER PROJECTS
  // =========================================================

  loadProjects(): void {

    this.errorMessage = '';

    this.projectService
      .getProjectsByOwner(this.ownerId)
      .subscribe({

        next: (data: Project[]) => {

          console.log(
            'Projects loaded:',
            data
          );

          this.projects = data || [];

          if (this.projects.length === 0) {

            this.selectedProject = null;
            this.issues = [];

            this.errorMessage =
              'No projects found for this owner.';

            return;
          }

          // Automatically select first project
          this.selectedProject =
            this.projects[0];

          this.loadIssues();

        },

        error: (error: any) => {

          console.error(
            'Failed to load projects:',
            error
          );

          this.projects = [];
          this.selectedProject = null;
          this.issues = [];

          this.errorMessage =
            'Failed to load projects.';
        }

      });
  }

  // =========================================================
  // SELECT PROJECT
  // =========================================================

  selectProject(
    project: Project
  ): void {

    this.selectedProject = project;

    this.selectedFilter = 'ALL';

    this.issues = [];

    this.loadIssues();
  }

  // =========================================================
  // LOAD ISSUES FOR SELECTED PROJECT
  // =========================================================

  loadIssues(): void {

    if (
      this.selectedProject === null ||
      this.selectedProject.id === undefined
    ) {

      this.issues = [];

      return;
    }

    const projectId =
      this.selectedProject.id;

    console.log(
      'Loading issues for project:',
      projectId
    );

    this.issueService
      .getIssuesByProject(projectId)
      .subscribe({

        next: (data: Issue[]) => {

          console.log(
            'Issues loaded:',
            data
          );

          this.issues = data || [];

          this.errorMessage = '';
        },

        error: (error: any) => {

          console.error(
            'Failed to load issues:',
            error
          );

          this.issues = [];

          this.errorMessage =
            'Failed to load issues.';
        }

      });
  }

  // =========================================================
  // REFRESH
  // =========================================================

  refreshDashboard(): void {

    this.loadProjects();
  }

  // =========================================================
  // FILTER
  // =========================================================

  filterIssues(
    filter: string
  ): void {

    this.selectedFilter = filter;
  }

  get filteredIssues(): Issue[] {

    switch (this.selectedFilter) {

      case 'OPEN':

        return this.issues.filter(
          issue =>
            issue.status === 'OPEN'
        );

      case 'IN_PROGRESS':

        return this.issues.filter(
          issue =>
            issue.status === 'IN_PROGRESS'
        );

      case 'HIGH':

        return this.issues.filter(
          issue =>
            issue.priority === 'HIGH'
        );

      case 'MEDIUM':

        return this.issues.filter(
          issue =>
            issue.priority === 'MEDIUM'
        );

      case 'CLOSED':

        return this.issues.filter(
          issue =>
            issue.status === 'CLOSED'
        );

      case 'ALL':

      default:

        return this.issues;
    }
  }

  // =========================================================
  // STATISTICS
  // =========================================================

  getTotalIssues(): number {

    return this.issues.length;
  }

  getOpenIssues(): number {

    return this.issues.filter(
      issue =>
        issue.status === 'OPEN'
    ).length;
  }

  getHighPriorityIssues(): number {

    return this.issues.filter(
      issue =>
        issue.priority === 'HIGH'
    ).length;
  }

  getInProgressIssues(): number {

    return this.issues.filter(
      issue =>
        issue.status === 'IN_PROGRESS'
    ).length;
  }

  getClosedIssues(): number {

    return this.issues.filter(
      issue =>
        issue.status === 'CLOSED'
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

    const status =
      select.value;

    if (!status) {

      alert(
        'Please select a valid status.'
      );

      return;
    }

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

        error: (error: any) => {

          console.error(
            'Failed to update status:',
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

    const priority =
      select.value;

    if (!priority) {

      alert(
        'Please select a valid priority.'
      );

      return;
    }

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

        error: (error: any) => {

          console.error(
            'Failed to update priority:',
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

    if (
      issue.id === undefined ||
      issue.id === null
    ) {

      alert('Issue ID is missing.');

      return;
    }

    const select =
      event.target as HTMLSelectElement;

    const assigneeId =
      Number(select.value);

    if (
      !Number.isInteger(assigneeId) ||
      assigneeId <= 0
    ) {

      alert(
        'Please select a valid assignee.'
      );

      return;
    }

    this.issueService
      .updateIssueAssignee(
        issue.id,
        assigneeId
      )
      .subscribe({

        next: (updatedIssue: Issue) => {

          issue.assigneeId =
            updatedIssue.assigneeId;

        },

        error: (error: any) => {

          console.error(
            'Failed to update assignee:',
            error
          );

          alert(
            'Failed to update issue assignee.'
          );
        }

      });
  }

  // =========================================================
  // STATUS CLASS
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
  // PRIORITY CLASS
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
