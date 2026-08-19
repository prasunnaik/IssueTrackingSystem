<div class="dashboard">

  <!-- ==================== HEADER ==================== -->

  <div class="dashboard-header">

    <div class="header-title">
      <h1>Project Owner Dashboard</h1>

      <p class="subtitle">
        Manage your projects and track issues
      </p>
    </div>

    <div class="header-buttons">

      <button
        class="create-project-btn"
        type="button"
        (click)="createProject()">

        + Create Project

      </button>

      <button
        class="create-issue-btn"
        type="button"
        (click)="createIssue()">

        + Create Issue

      </button>

      <button
        class="refresh-btn"
        type="button"
        (click)="refreshDashboard()">

        Refresh

      </button>

    </div>

  </div>


  <!-- ==================== ERROR ==================== -->

  <div
    class="error-message"
    *ngIf="errorMessage">

    {{ errorMessage }}

  </div>


  <!-- ==================== SUCCESS ==================== -->

  <div
    class="success-message"
    *ngIf="successMessage">

    {{ successMessage }}

  </div>


  <!-- ==================== NO PROJECTS ==================== -->

  <div
    class="empty-message"
    *ngIf="
      projects.length === 0 &&
      !errorMessage
    ">

    No projects found.

  </div>


  <!-- ==================== PROJECT SELECTOR ==================== -->

  <div
    class="project-selector"
    *ngIf="
      projects.length > 0 &&
      !editMode
    ">

    <label for="projectSelect">
      Select Project:
    </label>

    <select
      id="projectSelect"
      [ngModel]="selectedProject"
      (ngModelChange)="selectProject($event)">

      <option
        *ngFor="let project of projects"
        [ngValue]="project">

        {{ project.projectName }}

      </option>

    </select>

  </div>


  <!-- ==================== EDIT PROJECT ==================== -->

  <div
    class="edit-project-card"
    *ngIf="editMode">

    <h2>
      Edit Project
    </h2>


    <div class="form-group">

      <label>
        Project Name
      </label>

      <input
        type="text"
        [(ngModel)]="editProjectData.projectName"
        placeholder="Project name"
      />

    </div>


    <div class="form-group">

      <label>
        Product Owner ID
      </label>

      <input
        type="number"
        [(ngModel)]="editProjectData.productOwnerId"
        min="1"
      />

    </div>


    <div class="form-group">

      <label>
        Start Date
      </label>

      <input
        type="date"
        [(ngModel)]="editProjectData.startDate"
      />

    </div>


    <div class="form-group">

      <label>
        End Date
      </label>

      <input
        type="date"
        [(ngModel)]="editProjectData.endDate"
      />

    </div>


    <div class="edit-buttons">

      <button
        class="save-button"
        type="button"
        (click)="saveProject()">

        Save Changes

      </button>


      <button
        class="cancel-button"
        type="button"
        (click)="cancelEditProject()">

        Cancel

      </button>

    </div>

  </div>


  <!-- ==================== SELECTED PROJECT ==================== -->

  <div
    class="project-card"
    *ngIf="
      selectedProject &&
      !editMode
    ">


    <!-- PROJECT HEADER -->

    <div class="project-header">

      <div>

        <h2>
          {{ selectedProject.projectName }}
        </h2>

        <p>
          <strong>Project ID:</strong>
          {{ selectedProject.id }}
        </p>

        <p>
          <strong>Owner ID:</strong>
          {{ selectedProject.productOwnerId }}
        </p>

      </div>


      <div class="project-dates">

        <p>
          <strong>Start:</strong>
          {{ selectedProject.startDate }}
        </p>

        <p>
          <strong>End:</strong>
          {{ selectedProject.endDate }}
        </p>

      </div>

    </div>


    <!-- ==================== PROJECT ACTIONS ==================== -->

    <div class="project-actions">

      <button
        class="edit-button"
        type="button"
        (click)="startEditProject()">

        Edit Project

      </button>


      <button
        class="delete-button"
        type="button"
        (click)="deleteProject()">

        Delete Project

      </button>

    </div>


    <!-- ==================== STATISTICS ==================== -->

    <div class="stats">

      <div class="stat-card">

        <div class="stat-number">
          {{ getTotalIssues() }}
        </div>

        <div class="stat-label">
          Total Issues
        </div>

      </div>


      <div class="stat-card">

        <div class="stat-number">
          {{ getOpenIssues() }}
        </div>

        <div class="stat-label">
          Open Issues
        </div>

      </div>


      <div class="stat-card">

        <div class="stat-number">
          {{ getHighPriorityIssues() }}
        </div>

        <div class="stat-label">
          High Priority
        </div>

      </div>


      <div class="stat-card">

        <div class="stat-number">
          {{ getInProgressIssues() }}
        </div>

        <div class="stat-label">
          In Progress
        </div>

      </div>


      <div class="stat-card">

        <div class="stat-number">
          {{ getClosedIssues() }}
        </div>

        <div class="stat-label">
          Closed
        </div>

      </div>

    </div>


    <!-- ==================== FILTERS ==================== -->

    <div class="filters">

      <button
        type="button"
        [class.active]="selectedFilter === 'ALL'"
        (click)="filterIssues('ALL')">

        All

      </button>


      <button
        type="button"
        [class.active]="selectedFilter === 'OPEN'"
        (click)="filterIssues('OPEN')">

        Open

      </button>


      <button
        type="button"
        [class.active]="selectedFilter === 'IN_PROGRESS'"
        (click)="filterIssues('IN_PROGRESS')">

        In Progress

      </button>


      <button
        type="button"
        [class.active]="selectedFilter === 'HIGH'"
        (click)="filterIssues('HIGH')">

        High Priority

      </button>


      <button
        type="button"
        [class.active]="selectedFilter === 'MEDIUM'"
        (click)="filterIssues('MEDIUM')">

        Medium

      </button>


      <button
        type="button"
        [class.active]="selectedFilter === 'CLOSED'"
        (click)="filterIssues('CLOSED')">

        Closed

      </button>

    </div>


    <!-- ==================== ISSUES ==================== -->

    <div class="issues-section">

      <h2>
        Issues
      </h2>


      <div
        class="no-issues"
        *ngIf="filteredIssues.length === 0">

        No issues found for this project.

      </div>


      <!-- ==================== ISSUE CARD ==================== -->

      <div
        class="issue-card"
        *ngFor="let issue of filteredIssues">


        <!-- ISSUE HEADER -->

        <div class="issue-top">

          <div>

            <h3
              class="issue-title"
              (click)="openIssueDetails(issue)">

              {{ issue.summary }}

            </h3>

            <p class="issue-id">

              Issue ID:
              {{ issue.id }}

            </p>

          </div>


          <span
            class="status-badge"
            [ngClass]="getStatusClass(issue.status)">

            {{ issue.status }}

          </span>

        </div>


        <!-- DESCRIPTION -->

        <p class="description">

          <strong>Description:</strong>

          {{ issue.description }}

        </p>


        <!-- ISSUE DETAILS -->

        <div class="issue-details">


          <!-- PRIORITY -->

          <span>

            <strong>Priority:</strong>

            <select
              [ngModel]="issue.priority"
              (change)="
                updatePriority(
                  issue,
                  $event
                )
              ">

              <option value="HIGH">
                HIGH
              </option>

              <option value="MEDIUM">
                MEDIUM
              </option>

              <option value="LOW">
                LOW
              </option>

            </select>

          </span>


          <!-- TYPE -->

          <span>

            <strong>Type:</strong>

            {{ issue.type }}

          </span>


          <!-- ASSIGNEE -->

          <span>

            <strong>Assignee:</strong>

            <select
              [ngModel]="issue.assigneeId"
              (change)="
                updateAssignee(
                  issue,
                  $event
                )
              ">

              <option [ngValue]="1">
                1
              </option>

              <option [ngValue]="2">
                2
              </option>

              <option [ngValue]="3">
                3
              </option>

              <option [ngValue]="4">
                4
              </option>

            </select>

          </span>


          <!-- STORY POINTS -->

          <span>

            <strong>Story Points:</strong>

            {{ issue.storyPoint }}

          </span>

        </div>


        <!-- STATUS -->

        <div class="status-update">

          <label>
            Update Status:
          </label>

          <select
            [ngModel]="issue.status"
            (change)="
              updateStatus(
                issue,
                $event
              )
            ">

            <option value="OPEN">
              OPEN
            </option>

            <option value="IN_PROGRESS">
              IN_PROGRESS
            </option>

            <option value="CLOSED">
              CLOSED
            </option>

          </select>

        </div>

      </div>

    </div>

  </div>

</div>
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

  ownerId: number = 0;
  ownerName: string = '';


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

  const storedUser = localStorage.getItem('user');

  if (!storedUser) {

    this.router.navigate(['/login']);

    return;
  }

  const user = JSON.parse(storedUser);

  this.ownerId = Number(user.id);
  this.ownerName = user.name || 'Project Owner';

  if (!this.ownerId) {

    this.errorMessage =
      'Logged-in user information is missing.';

    return;
  }

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

  // Create Issue//

  createIssue(): void {
    this.router.navigate([
      '/create-issue'
    ]);
  }

  openIssueDetails(issue: Issue): void {

  if (!issue.id) {
    alert('Issue ID is missing.');
    return;
  }

  this.router.navigate([
    '/issue',
    issue.id
  ]);
}
}
/* =========================================================
   MAIN DASHBOARD
   ========================================================= */

.dashboard {
  max-width: 1100px;
  margin: 40px auto;
  padding: 30px;

  font-family: Arial, Helvetica, sans-serif;

  color: #333;
  background-color: #f7f9fc;

  min-height: 100vh;
}


/* =========================================================
   HEADER
   ========================================================= */

.dashboard-header {
  display: flex;

  justify-content: space-between;
  align-items: center;

  gap: 20px;

  margin-bottom: 30px;
}


.header-title {
  flex: 1;
}


.dashboard h1 {
  margin: 0 0 8px 0;

  font-size: 32px;

  color: #222;
}


.subtitle {
  margin: 0;

  color: #777;

  font-size: 15px;
}


/* =========================================================
   HEADER BUTTONS
   ========================================================= */

.header-buttons {
  display: flex;

  align-items: center;

  gap: 10px;

  flex-wrap: wrap;
}


.create-project-btn,
.create-issue-btn,
.refresh-btn {
  padding: 10px 16px;

  border: none;

  border-radius: 6px;

  color: white;

  font-size: 14px;

  font-weight: 600;

  cursor: pointer;
}


/* Create Project */

.create-project-btn {
  background-color: #1976d2;
}


.create-project-btn:hover {
  background-color: #125ea8;
}


/* Create Issue */

.create-issue-btn {
  background-color: #2e7d32;
}


.create-issue-btn:hover {
  background-color: #1b5e20;
}


/* Refresh */

.refresh-btn {
  background-color: #616161;
}


.refresh-btn:hover {
  background-color: #424242;
}


/* =========================================================
   ERROR
   ========================================================= */

.error-message {
  padding: 12px 16px;

  margin-bottom: 20px;

  border-radius: 6px;

  background-color: #ffebee;

  color: #c62828;

  border: 1px solid #ffcdd2;
}


/* =========================================================
   SUCCESS
   ========================================================= */

.success-message {
  padding: 12px 16px;

  margin-bottom: 20px;

  border-radius: 6px;

  background-color: #e8f5e9;

  color: #2e7d32;

  border: 1px solid #c8e6c9;
}


/* =========================================================
   PROJECT SELECTOR
   ========================================================= */

.project-selector {
  display: flex;

  align-items: center;

  gap: 12px;

  margin-bottom: 20px;

  padding: 15px;

  background-color: white;

  border: 1px solid #e0e0e0;

  border-radius: 8px;
}


.project-selector label {
  font-weight: 600;
}


.project-selector select {
  min-width: 250px;

  padding: 9px 12px;

  border: 1px solid #ccc;

  border-radius: 6px;

  background-color: white;

  cursor: pointer;
}


/* =========================================================
   EMPTY MESSAGE
   ========================================================= */

.empty-message,
.no-projects,
.no-issues {
  text-align: center;

  padding: 30px;

  color: #777;

  background-color: white;

  border-radius: 10px;

  border: 1px solid #e0e0e0;
}


/* =========================================================
   EDIT PROJECT
   ========================================================= */

.edit-project-card {
  max-width: 700px;

  margin: 20px auto;

  padding: 25px;

  background-color: white;

  border-radius: 10px;

  border: 1px solid #e0e0e0;

  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.08);
}


.edit-project-card h2 {
  margin-top: 0;

  margin-bottom: 25px;
}


.form-group {
  margin-bottom: 18px;
}


.form-group label {
  display: block;

  margin-bottom: 6px;

  font-weight: 600;
}


.form-group input {
  width: 100%;

  padding: 10px;

  box-sizing: border-box;

  border: 1px solid #ccc;

  border-radius: 5px;

  font-size: 14px;
}


.form-group input:focus {
  outline: none;

  border-color: #1976d2;
}


/* Edit Buttons */

.edit-buttons {
  display: flex;

  gap: 10px;

  margin-top: 20px;
}


.save-button,
.cancel-button {
  padding: 9px 16px;

  border: none;

  border-radius: 5px;

  cursor: pointer;

  font-weight: 600;
}


.save-button {
  background-color: #1976d2;

  color: white;
}


.save-button:hover {
  background-color: #125ea8;
}


.cancel-button {
  background-color: #ddd;

  color: #333;
}


.cancel-button:hover {
  background-color: #ccc;
}


/* =========================================================
   PROJECT CARD
   ========================================================= */

.project-card {
  background-color: white;

  border: 1px solid #e0e0e0;

  border-radius: 12px;

  padding: 25px;

  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);

  margin-bottom: 30px;
}


/* =========================================================
   PROJECT HEADER
   ========================================================= */

.project-header {
  display: flex;

  justify-content: space-between;

  align-items: flex-start;

  padding-bottom: 20px;

  border-bottom: 1px solid #e5e5e5;
}


.project-header h2 {
  margin: 0 0 8px 0;

  font-size: 24px;

  color: #333;
}


.project-header p {
  margin: 5px 0;

  color: #666;
}


.project-dates {
  text-align: right;
}


.project-dates p {
  margin: 5px 0;
}


/* =========================================================
   PROJECT ACTIONS
   ========================================================= */

.project-actions {
  display: flex;

  gap: 10px;

  margin: 20px 0;
}


.edit-button,
.delete-button {
  padding: 9px 16px;

  border: none;

  border-radius: 5px;

  cursor: pointer;

  font-weight: 600;
}


.edit-button {
  background-color: #1976d2;

  color: white;
}


.edit-button:hover {
  background-color: #125ea8;
}


.delete-button {
  background-color: #d32f2f;

  color: white;
}


.delete-button:hover {
  background-color: #b71c1c;
}


/* =========================================================
   STATISTICS
   ========================================================= */

.stats {
  display: grid;

  grid-template-columns:
    repeat(5, 1fr);

  gap: 15px;

  margin: 25px 0;
}


.stat-card {
  border: 1px solid #e0e0e0;

  border-radius: 8px;

  padding: 18px;

  text-align: center;

  background-color: #fafafa;
}


.stat-number {
  font-size: 28px;

  font-weight: bold;

  color: #1976d2;

  margin-bottom: 5px;
}


.stat-label {
  color: #777;

  font-size: 13px;
}


/* =========================================================
   FILTERS
   ========================================================= */

.filters {
  display: flex;

  flex-wrap: wrap;

  gap: 8px;

  margin: 25px 0;
}


.filters button {
  padding: 8px 14px;

  border: 1px solid #ccc;

  border-radius: 6px;

  background-color: white;

  color: #555;

  cursor: pointer;

  font-size: 13px;
}


.filters button:hover {
  background-color: #f0f0f0;
}


.filters button.active {
  background-color: #1976d2;

  color: white;

  border-color: #1976d2;
}


/* =========================================================
   ISSUES SECTION
   ========================================================= */

.issues-section {
  margin-top: 25px;
}


.issues-section h2 {
  font-size: 20px;

  margin-bottom: 15px;

  color: #333;
}


/* =========================================================
   ISSUE CARD
   ========================================================= */

.issue-card {
  border: 1px solid #e1e1e1;

  border-radius: 8px;

  padding: 20px;

  margin-bottom: 15px;

  background-color: #fff;
}


.issue-card:hover {
  box-shadow:
    0 2px 8px rgba(0, 0, 0, 0.06);
}


/* =========================================================
   ISSUE TOP
   ========================================================= */

.issue-top {
  display: flex;

  justify-content: space-between;

  align-items: center;

  gap: 15px;

  margin-bottom: 15px;
}


.issue-title {
  margin: 0;

  font-size: 18px;

  color: #1976d2;

  cursor: pointer;
}


.issue-title:hover {
  text-decoration: underline;
}


.issue-id {
  margin: 5px 0 0 0;

  font-size: 12px;

  color: #888;
}


/* =========================================================
   STATUS BADGE
   ========================================================= */

.status-badge {
  padding: 6px 12px;

  border-radius: 20px;

  font-size: 12px;

  font-weight: bold;

  white-space: nowrap;
}


/* Open */

.status-badge.OPEN {
  background-color: #e3f2fd;

  color: #1565c0;
}


/* In Progress */

.status-badge.IN_PROGRESS {
  background-color: #fff3e0;

  color: #ef6c00;
}


/* Closed */

.status-badge.CLOSED {
  background-color: #e8f5e9;

  color: #2e7d32;
}


/* =========================================================
   DESCRIPTION
   ========================================================= */

.description {
  margin: 8px 0;

  font-size: 14px;

  color: #555;

  line-height: 1.5;
}


/* =========================================================
   ISSUE DETAILS
   ========================================================= */

.issue-details {
  display: flex;

  flex-wrap: wrap;

  gap: 20px;

  margin-top: 15px;

  padding-top: 15px;

  border-top: 1px solid #eeeeee;
}


.issue-details span {
  font-size: 13px;

  color: #555;
}


.issue-details strong {
  margin-right: 5px;
}


.issue-details select {
  padding: 5px 8px;

  border: 1px solid #ccc;

  border-radius: 5px;

  background-color: white;

  cursor: pointer;
}


/* =========================================================
   STATUS UPDATE
   ========================================================= */

.status-update {
  display: flex;

  align-items: center;

  gap: 12px;

  margin-top: 20px;

  padding-top: 15px;

  border-top: 1px solid #eeeeee;
}


.status-update label {
  font-weight: 600;

  font-size: 14px;
}


.status-update select {
  padding: 8px 12px;

  border: 1px solid #ccc;

  border-radius: 6px;

  background-color: white;

  font-size: 14px;

  cursor: pointer;
}


.status-update select:focus,
.issue-details select:focus {
  outline: none;

  border-color: #1976d2;
}


/* =========================================================
   RESPONSIVE
   ========================================================= */

@media (max-width: 900px) {

  .stats {
    grid-template-columns:
      repeat(3, 1fr);
  }

}


@media (max-width: 768px) {

  .dashboard {
    margin: 10px;

    padding: 15px;
  }


  .dashboard-header {
    flex-direction: column;

    align-items: stretch;
  }


  .dashboard h1 {
    text-align: center;

    font-size: 26px;
  }


  .subtitle {
    text-align: center;
  }


  .header-buttons {
    justify-content: center;
  }


  .project-header {
    flex-direction: column;
  }


  .project-dates {
    text-align: left;

    margin-top: 15px;
  }


  .stats {
    grid-template-columns: 1fr;
  }


  .project-selector {
    flex-direction: column;

    align-items: stretch;
  }


  .project-selector select {
    width: 100%;

    min-width: unset;
  }


  .issue-top {
    flex-direction: column;

    align-items: flex-start;
  }


  .issue-details {
    flex-direction: column;

    gap: 10px;
  }


  .status-update {
    flex-direction: column;

    align-items: flex-start;
  }


  .project-actions {
    flex-wrap: wrap;
  }

}


@media (max-width: 500px) {

  .header-buttons {
    flex-direction: column;

    width: 100%;
  }


  .create-project-btn,
  .create-issue-btn,
  .refresh-btn {
    width: 100%;
  }


  .project-actions button {
    width: 100%;
  }

}
