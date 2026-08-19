<div class="app-layout">

  <!-- =========================================================
       LEFT SIDEBAR
       ========================================================= -->

  <aside class="sidebar">

    <!-- PROFILE -->

    <div class="profile-section">

      <img
        class="profile-image"
        [src]="profileImage || 'assets/default-profile.png'"
        alt="Project Owner Profile"
      />

      <h3>
        {{ ownerName }}
      </h3>

      <p class="profile-email">
        {{ ownerEmail }}
      </p>


      <!-- PROFILE STATS -->

      <div class="profile-stats">

        <div class="profile-stat">

          <strong>
            {{ projects.length }}
          </strong>

          <span>
            Projects
          </span>

        </div>


        <div class="profile-stat">

          <strong>
            {{ issues.length }}
          </strong>

          <span>
            Issues
          </span>

        </div>

      </div>

    </div>


    <!-- SIDEBAR MENU -->

    <nav class="sidebar-menu">

      <button
        type="button"
        class="sidebar-link active"
        (click)="refreshDashboard()">

        <span class="menu-icon">
          📊
        </span>

        Project Dashboard

      </button>


      <button
        type="button"
        class="sidebar-link"
        (click)="createProject()">

        <span class="menu-icon">
          📁
        </span>

        Create Project

      </button>


      <button
        type="button"
        class="sidebar-link"
        (click)="createIssue()">

        <span class="menu-icon">
          📝
        </span>

        Create Issue

      </button>

    </nav>


    <!-- LOGOUT -->

    <button
      type="button"
      class="sidebar-logout"
      (click)="logout()">

      Logout

    </button>

  </aside>



  <!-- =========================================================
       MAIN CONTENT
       ========================================================= -->

  <main class="main-content">


    <!-- =======================================================
         TOP APPLICATION HEADER
         ======================================================= -->

    <header class="app-header">

      <!-- SEARCH -->

      <div class="search-container">

        <input
          type="text"
          [(ngModel)]="searchText"
          placeholder="Search issue by summary or description"
        />

        <button
          type="button"
          class="search-button">

          🔍

        </button>

      </div>


      <!-- APPLICATION NAME -->

      <div class="application-name">

        Issue Tracking System

      </div>

    </header>



    <!-- =======================================================
         DASHBOARD CONTENT
         ======================================================= -->

    <div class="dashboard">


      <!-- PAGE TITLE -->

      <div class="dashboard-heading">

        <div>

          <h1>
            Project Dashboard
          </h1>

          <p class="subtitle">
            Manage your projects and track issues
          </p>

        </div>


        <button
          type="button"
          class="refresh-main-btn"
          (click)="refreshDashboard()">

          Refresh

        </button>

      </div>



      <!-- =====================================================
           ERROR
           ===================================================== -->

      <div
        class="error-message"
        *ngIf="errorMessage">

        {{ errorMessage }}

      </div>



      <!-- =====================================================
           SUCCESS
           ===================================================== -->

      <div
        class="success-message"
        *ngIf="successMessage">

        {{ successMessage }}

      </div>



      <!-- =====================================================
           NO PROJECTS
           ===================================================== -->

      <div
        class="no-projects-screen"
        *ngIf="
          projects.length === 0 &&
          !errorMessage
        ">

        <div class="no-projects-icon">
          📁
        </div>

        <h2>
          No Projects Available
        </h2>

        <p>
          You don't have any projects yet.
        </p>

        <button
          type="button"
          class="create-project-empty-btn"
          (click)="createProject()">

          Create Project

        </button>

      </div>



      <!-- =====================================================
           PROJECT AREA
           ===================================================== -->

      <ng-container
        *ngIf="
          projects.length > 0 &&
          !editMode
        ">


        <!-- PROJECT SELECTOR -->

        <div class="project-selector">

          <div class="selector-left">

            <label for="projectSelect">
              Project Name
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


          <div class="owner-name-display">

            <span>
              Project Owner:
            </span>

            <strong>
              {{ ownerName }}
            </strong>

          </div>

        </div>



        <!-- =================================================
             SELECTED PROJECT
             ================================================= -->

        <div
          class="project-card"
          *ngIf="selectedProject">


          <!-- PROJECT INFORMATION -->

          <div class="project-information">

            <div class="project-info-left">

              <h2>
                {{ selectedProject.projectName }}
              </h2>

              <p>
                <strong>Project ID:</strong>
                {{ selectedProject.id }}
              </p>

              <p>
                <strong>Project Owner:</strong>
                {{ ownerName }}
              </p>

            </div>


            <div class="project-dates">

              <div>

                <span>
                  Start Date
                </span>

                <strong>
                  {{ selectedProject.startDate }}
                </strong>

              </div>


              <div>

                <span>
                  End Date
                </span>

                <strong>
                  {{ selectedProject.endDate }}
                </strong>

              </div>

            </div>

          </div>



          <!-- PROJECT ACTIONS -->

          <div class="project-actions">

            <button
              type="button"
              class="edit-button"
              (click)="startEditProject()">

              Edit Project

            </button>


            <button
              type="button"
              class="delete-button"
              (click)="deleteProject()">

              Delete Project

            </button>

          </div>



          <!-- =================================================
               STATISTICS
               ================================================= -->

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
                Open
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
                {{ getHighPriorityIssues() }}
              </div>

              <div class="stat-label">
                High Priority
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



          <!-- =================================================
               FILTERS
               ================================================= -->

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



          <!-- =================================================
               ISSUES
               ================================================= -->

          <div class="issues-section">

            <div class="issues-heading">

              <h2>
                Issues
              </h2>

              <span>
                {{ filteredIssues.length }} issues
              </span>

            </div>



            <!-- NO ISSUES -->

            <div
              class="no-issues"
              *ngIf="filteredIssues.length === 0">

              No issues found for this project.

            </div>



            <!-- =================================================
                 KANBAN BOARD
                 ================================================= -->

            <div
              class="kanban-board"
              *ngIf="filteredIssues.length > 0">


              <!-- OPEN -->

              <div class="kanban-column">

                <div class="kanban-column-header open-header">

                  <span>
                    TO DO
                  </span>

                  <span class="column-count">
                    {{ getBoardIssues('OPEN').length }}
                  </span>

                </div>


                <div class="kanban-cards">

                  <div
                    class="kanban-issue"
                    *ngFor="
                      let issue of getBoardIssues('OPEN')
                    ">

                    <div class="issue-card-header">

                      <span class="issue-number">
                        ID: {{ issue.id }}
                      </span>

                      <span
                        class="priority-badge"
                        [ngClass]="
                          getPriorityClass(
                            issue.priority
                          )
                        ">

                        {{ issue.priority }}

                      </span>

                    </div>


                    <h3
                      (click)="openIssueDetails(issue)">

                      {{ issue.summary }}

                    </h3>


                    <p class="issue-description">

                      {{ issue.description }}

                    </p>


                    <div class="issue-meta">

                      <span>
                        Type: {{ issue.type }}
                      </span>

                      <span>
                        Points: {{ issue.storyPoint }}
                      </span>

                    </div>


                    <div class="issue-assignee">

                      <span>
                        Assignee:
                      </span>

                      <strong>
                        #{{ issue.assigneeId }}
                      </strong>

                    </div>


                    <div class="card-controls">

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
                          IN PROGRESS
                        </option>

                        <option value="CLOSED">
                          CLOSED
                        </option>

                      </select>

                    </div>

                  </div>

                </div>

              </div>



              <!-- IN PROGRESS -->

              <div class="kanban-column">

                <div class="kanban-column-header progress-header">

                  <span>
                    DEVELOPMENT
                  </span>

                  <span class="column-count">
                    {{ getBoardIssues('IN_PROGRESS').length }}
                  </span>

                </div>


                <div class="kanban-cards">

                  <div
                    class="kanban-issue"
                    *ngFor="
                      let issue of getBoardIssues(
                        'IN_PROGRESS'
                      )
                    ">

                    <div class="issue-card-header">

                      <span class="issue-number">
                        ID: {{ issue.id }}
                      </span>

                      <span
                        class="priority-badge"
                        [ngClass]="
                          getPriorityClass(
                            issue.priority
                          )
                        ">

                        {{ issue.priority }}

                      </span>

                    </div>


                    <h3
                      (click)="openIssueDetails(issue)">

                      {{ issue.summary }}

                    </h3>


                    <p class="issue-description">

                      {{ issue.description }}

                    </p>


                    <div class="issue-meta">

                      <span>
                        Type: {{ issue.type }}
                      </span>

                      <span>
                        Points: {{ issue.storyPoint }}
                      </span>

                    </div>


                    <div class="issue-assignee">

                      <span>
                        Assignee:
                      </span>

                      <strong>
                        #{{ issue.assigneeId }}
                      </strong>

                    </div>


                    <div class="card-controls">

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
                          IN PROGRESS
                        </option>

                        <option value="CLOSED">
                          CLOSED
                        </option>

                      </select>

                    </div>

                  </div>

                </div>

              </div>



              <!-- CLOSED -->

              <div class="kanban-column">

                <div class="kanban-column-header closed-header">

                  <span>
                    COMPLETED
                  </span>

                  <span class="column-count">
                    {{ getBoardIssues('CLOSED').length }}
                  </span>

                </div>


                <div class="kanban-cards">

                  <div
                    class="kanban-issue"
                    *ngFor="
                      let issue of getBoardIssues('CLOSED')
                    ">

                    <div class="issue-card-header">

                      <span class="issue-number">
                        ID: {{ issue.id }}
                      </span>

                      <span
                        class="priority-badge"
                        [ngClass]="
                          getPriorityClass(
                            issue.priority
                          )
                        ">

                        {{ issue.priority }}

                      </span>

                    </div>


                    <h3
                      (click)="openIssueDetails(issue)">

                      {{ issue.summary }}

                    </h3>


                    <p class="issue-description">

                      {{ issue.description }}

                    </p>


                    <div class="issue-meta">

                      <span>
                        Type: {{ issue.type }}
                      </span>

                      <span>
                        Points: {{ issue.storyPoint }}
                      </span>

                    </div>


                    <div class="issue-assignee">

                      <span>
                        Assignee:
                      </span>

                      <strong>
                        #{{ issue.assigneeId }}
                      </strong>

                    </div>


                    <div class="card-controls">

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
                          IN PROGRESS
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

          </div>

        </div>

      </ng-container>



      <!-- =====================================================
           EDIT PROJECT
           ===================================================== -->

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


    </div>

  </main>

</div>
