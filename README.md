<div class="dashboard">

  <!-- HEADER -->
  <div class="dashboard-header">

    <div>
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
        class="refresh-btn"
        type="button"
        (click)="refreshDashboard()">

        Refresh

      </button>

    </div>

  </div>


  <!-- ERROR -->
  <div
    class="error-message"
    *ngIf="errorMessage">

    {{ errorMessage }}

  </div>


  <!-- SUCCESS -->
  <div
    class="success-message"
    *ngIf="successMessage">

    {{ successMessage }}

  </div>


  <!-- NO PROJECTS -->
  <div
    class="empty-message"
    *ngIf="projects.length === 0 && !errorMessage">

    No projects found.

  </div>


  <!-- PROJECT SELECTOR -->
  <div
    class="project-selector"
    *ngIf="projects.length > 0 && !editMode">

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


  <!-- ===================================================== -->
  <!-- EDIT PROJECT -->
  <!-- ===================================================== -->

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
        [(ngModel)]="
          editProjectData.projectName
        "
        placeholder="Project name"
      />

    </div>


    <div class="form-group">

      <label>
        Product Owner ID
      </label>

      <input
        type="number"
        [(ngModel)]="
          editProjectData.productOwnerId
        "
        min="1"
      />

    </div>


    <div class="form-group">

      <label>
        Start Date
      </label>

      <input
        type="date"
        [(ngModel)]="
          editProjectData.startDate
        "
      />

    </div>


    <div class="form-group">

      <label>
        End Date
      </label>

      <input
        type="date"
        [(ngModel)]="
          editProjectData.endDate
        "
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


  <!-- ===================================================== -->
  <!-- SELECTED PROJECT -->
  <!-- ===================================================== -->

  <div
    class="project-card"
    *ngIf="selectedProject && !editMode">


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


    <!-- PROJECT ACTIONS -->

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


    <!-- STATISTICS -->

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


    <!-- FILTERS -->

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
        [class.active]="
          selectedFilter === 'IN_PROGRESS'
        "
        (click)="
          filterIssues('IN_PROGRESS')
        ">

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


    <!-- ISSUES -->

    <div class="issues-section">

      <h2>
        Issues
      </h2>


      <div
        class="no-issues"
        *ngIf="filteredIssues.length === 0">

        No issues found for this project.

      </div>


      <!-- ISSUE CARD -->

      <div
        class="issue-card"
        *ngFor="let issue of filteredIssues">


        <!-- ISSUE HEADER -->

        <div class="issue-top">

          <div>

            <h3>
              {{ issue.summary }}
            </h3>

            <p class="issue-id">
              Issue ID:
              {{ issue.id }}
            </p>

          </div>


          <span
            class="status-badge"
            [ngClass]="
              getStatusClass(issue.status)
            ">

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

              <option value="1">
                1
              </option>

              <option value="2">
                2
              </option>

              <option value="3">
                3
              </option>

              <option value="4">
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
