import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { Router } from '@angular/router';

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
  // EDIT PROJECT
  // =========================================================

  editMode: boolean = false;

  editProjectData: Project = {
    projectName: '',
    productOwnerId: 1,
    startDate: '',
    endDate: ''
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
  // ERROR / SUCCESS
  // =========================================================

  errorMessage: string = '';

  successMessage: string = '';


  // =========================================================
  // CONSTRUCTOR
  // =========================================================

  constructor(
    private projectService: ProjectService,
    private issueService: IssueService,
    private router: Router
  ) {}


  // =========================================================
  // INIT
  // =========================================================

  ngOnInit(): void {
    this.loadProjects();
  }


  // =========================================================
  // LOAD PROJECTS
  // =========================================================

  loadProjects(): void {

    this.errorMessage = '';

    this.projectService
      .getProjectsByOwner(this.ownerId)
      .subscribe({

        next: (data: Project[]) => {

          this.projects = data || [];

          if (this.projects.length > 0) {

            if (
              this.selectedProject &&
              this.selectedProject.id !== undefined
            ) {

              const existingProject =
                this.projects.find(
                  project =>
                    project.id ===
                    this.selectedProject?.id
                );

              if (existingProject) {

                this.selectedProject =
                  existingProject;

              } else {

                this.selectedProject =
                  this.projects[0];
              }

            } else {

              this.selectedProject =
                this.projects[0];
            }

            this.loadIssues();

          } else {

            this.selectedProject = null;
            this.issues = [];
          }
        },

        error: (error: any) => {

          console.error(
            'Failed to load projects:',
            error
          );

          this.errorMessage =
            'Failed to load projects.';

          this.projects = [];
          this.selectedProject = null;
          this.issues = [];
        }
      });
  }


  // =========================================================
  // SELECT PROJECT
  // =========================================================

  selectProject(project: Project): void {

    this.selectedProject = project;

    this.selectedFilter = 'ALL';

    this.issues = [];

    this.errorMessage = '';

    this.editMode = false;

    this.loadIssues();
  }


  // =========================================================
  // LOAD ISSUES
  // =========================================================

  loadIssues(): void {

    if (
      !this.selectedProject ||
      this.selectedProject.id === undefined
    ) {

      this.issues = [];

      return;
    }

    const projectId =
      this.selectedProject.id;

    this.issueService
      .getIssuesByProject(projectId)
      .subscribe({

        next: (data: Issue[]) => {

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

    this.editMode = false;

    this.successMessage = '';

    this.loadProjects();
  }


  // =========================================================
  // CREATE PROJECT
  // =========================================================

  createProject(): void {

    this.router.navigate([
      '/create-project'
    ]);
  }


  // =========================================================
  // START EDITING PROJECT
  // =========================================================

  startEditProject(): void {

    if (!this.selectedProject) {

      alert('Please select a project.');

      return;
    }

    this.errorMessage = '';
    this.successMessage = '';

    this.editProjectData = {
      id: this.selectedProject.id,
      projectName:
        this.selectedProject.projectName,
      productOwnerId:
        this.selectedProject.productOwnerId,
      startDate:
        this.selectedProject.startDate,
      endDate:
        this.selectedProject.endDate
    };

    this.editMode = true;
  }


  // =========================================================
  // CANCEL EDIT
  // =========================================================

  cancelEditProject(): void {

    this.editMode = false;

    this.errorMessage = '';
    this.successMessage = '';
  }


  // =========================================================
  // SAVE EDITED PROJECT
  // =========================================================

  saveProject(): void {

    if (
      !this.editProjectData.id
    ) {

      this.errorMessage =
        'Project ID is missing.';

      return;
    }

    if (
      !this.editProjectData.projectName ||
      !this.editProjectData.projectName.trim()
    ) {

      this.errorMessage =
        'Project name is required.';

      return;
    }

    if (
      !this.editProjectData.productOwnerId
    ) {

      this.errorMessage =
        'Product owner ID is required.';

      return;
    }

    if (
      !this.editProjectData.startDate
    ) {

      this.errorMessage =
        'Start date is required.';

      return;
    }

    if (
      !this.editProjectData.endDate
    ) {

      this.errorMessage =
        'End date is required.';

      return;
    }

    if (
      this.editProjectData.endDate <
      this.editProjectData.startDate
    ) {

      this.errorMessage =
        'End date cannot be before start date.';

      return;
    }

    this.errorMessage = '';
    this.successMessage = '';

    const projectId =
      this.editProjectData.id;

    this.projectService
      .updateProject(
        projectId,
        this.editProjectData
      )
      .subscribe({

        next: (updatedProject: Project) => {

          this.successMessage =
            'Project updated successfully.';

          this.editMode = false;

          this.selectedProject =
            updatedProject;

          this.loadProjects();
        },

        error: (error: any) => {

          console.error(
            'Failed to update project:',
            error
          );

          this.errorMessage =
            'Failed to update project.';
        }
      });
  }


  // =========================================================
  // DELETE PROJECT
  // =========================================================

  deleteProject(): void {

    if (
      !this.selectedProject ||
      this.selectedProject.id === undefined
    ) {

      alert('Please select a project.');

      return;
    }

    const projectName =
      this.selectedProject.projectName;

    const projectId =
      this.selectedProject.id;

    const confirmed =
      window.confirm(
        `Are you sure you want to delete "${projectName}"?`
      );

    if (!confirmed) {
      return;
    }

    this.errorMessage = '';
    this.successMessage = '';

    this.projectService
      .deleteProject(projectId)
      .subscribe({

        next: () => {

          this.successMessage =
            'Project deleted successfully.';

          this.selectedProject = null;

          this.issues = [];

          this.editMode = false;

          this.loadProjects();
        },

        error: (error: any) => {

          console.error(
            'Failed to delete project:',
            error
          );

          this.errorMessage =
            'Failed to delete project.';
        }
      });
  }


  // =========================================================
  // FILTER
  // =========================================================

  filterIssues(
    filter: string
  ): void {

    this.selectedFilter = filter;
  }


  // =========================================================
  // FILTERED ISSUES
  // =========================================================

  get filteredIssues(): Issue[] {

    if (
      this.selectedFilter === 'ALL'
    ) {

      return this.issues;
    }

    if (
      this.selectedFilter === 'OPEN'
    ) {

      return this.issues.filter(
        issue =>
          issue.status === 'OPEN'
      );
    }

    if (
      this.selectedFilter === 'IN_PROGRESS'
    ) {

      return this.issues.filter(
        issue =>
          issue.status === 'IN_PROGRESS'
      );
    }

    if (
      this.selectedFilter === 'HIGH'
    ) {

      return this.issues.filter(
        issue =>
          issue.priority === 'HIGH'
      );
    }

    if (
      this.selectedFilter === 'MEDIUM'
    ) {

      return this.issues.filter(
        issue =>
          issue.priority === 'MEDIUM'
      );
    }

    if (
      this.selectedFilter === 'CLOSED'
    ) {

      return this.issues.filter(
        issue =>
          issue.status === 'CLOSED'
      );
    }

    return this.issues;
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
      issue.id === undefined
    ) {

      alert('Issue ID is missing.');

      return;
    }

    const select =
      event.target as HTMLSelectElement;

    const status =
      select.value;

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
            'Failed to update issue status:',
            error
          );

          alert(
            'Failed to update issue status.'
          );

          this.loadIssues();
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
      issue.id === undefined
    ) {

      alert('Issue ID is missing.');

      return;
    }

    const select =
      event.target as HTMLSelectElement;

    const priority =
      select.value;

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
            'Failed to update issue priority:',
            error
          );

          alert(
            'Failed to update issue priority.'
          );

          this.loadIssues();
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
      issue.id === undefined
    ) {

      alert('Issue ID is missing.');

      return;
    }

    const select =
      event.target as HTMLSelectElement;

    const assigneeId =
      Number(select.value);

    if (!assigneeId) {

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
            'Failed to update issue assignee:',
            error
          );

          alert(
            'Failed to update issue assignee.'
          );

          this.loadIssues();
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
