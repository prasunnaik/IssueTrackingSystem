/* =========================================================
   GLOBAL LAYOUT
   ========================================================= */

* {
  box-sizing: border-box;
}


.app-layout {
  display: flex;
  min-height: 100vh;
  width: 100%;
  background: #f4f5f7;

  font-family:
    Arial,
    Helvetica,
    sans-serif;

  color: #333;
}


/* =========================================================
   SIDEBAR
   ========================================================= */

.sidebar {
  width: 220px;
  min-height: 100vh;

  position: fixed;

  left: 0;
  top: 0;
  bottom: 0;

  background: #20252b;
  color: white;

  padding: 20px 14px;

  z-index: 10;

  display: flex;
  flex-direction: column;
}


/* =========================================================
   PROFILE
   ========================================================= */

.profile-section {
  text-align: center;

  padding-bottom: 22px;

  border-bottom: 1px solid #343a40;
}


.profile-image {
  width: 78px;
  height: 78px;

  object-fit: cover;

  border-radius: 50%;

  border: 3px solid #ffffff;

  display: block;

  margin: 0 auto 10px auto;

  background: #eeeeee;
}


.profile-section h3 {
  margin: 5px 0;

  font-size: 16px;

  color: #ffffff;

  font-weight: 600;
}


.profile-email {
  margin: 0;

  color: #bfc3c7;

  font-size: 11px;

  word-break: break-word;
}


/* =========================================================
   PROFILE STATS
   ========================================================= */

.profile-stats {
  display: flex;

  justify-content: center;

  gap: 35px;

  margin-top: 18px;
}


.profile-stat {
  display: flex;

  flex-direction: column;

  align-items: center;
}


.profile-stat strong {
  font-size: 18px;

  color: #ffffff;
}


.profile-stat span {
  font-size: 10px;

  color: #bfc3c7;

  margin-top: 3px;
}


/* =========================================================
   SIDEBAR MENU
   ========================================================= */

.sidebar-menu {
  margin-top: 20px;
}


.sidebar-link {
  width: 100%;

  display: flex;

  align-items: center;

  gap: 8px;

  padding: 11px 8px;

  margin-bottom: 4px;

  border: none;

  border-radius: 4px;

  background: transparent;

  color: #f5c400;

  text-align: left;

  cursor: pointer;

  font-size: 12px;

  transition: background 0.2s ease;
}


.sidebar-link:hover {
  background: #30363d;
}


.sidebar-link.active {
  background: #2c3239;
}


.menu-icon {
  width: 18px;

  text-align: center;
}


/* =========================================================
   LOGOUT
   ========================================================= */

.sidebar-logout {
  margin-top: auto;

  width: 100%;

  padding: 8px;

  border: 1px solid #f5c400;

  border-radius: 5px;

  background: transparent;

  color: #f5c400;

  cursor: pointer;

  font-size: 11px;
}


.sidebar-logout:hover {
  background: #f5c400;

  color: #20252b;
}


/* =========================================================
   MAIN CONTENT
   ========================================================= */

.main-content {
  margin-left: 220px;

  width: calc(100% - 220px);

  min-height: 100vh;

  background: #f4f5f7;
}


/* =========================================================
   APPLICATION HEADER
   ========================================================= */

.app-header {
  height: 48px;

  width: 100%;

  background: #ffc400;

  display: flex;

  align-items: center;

  justify-content: space-between;

  padding: 0 20px;
}


.search-container {
  display: flex;

  width: 380px;

  height: 30px;
}


.search-container input {
  flex: 1;

  border: none;

  outline: none;

  padding: 0 10px;

  font-size: 11px;

  background: #ffffff;
}


.search-button {
  width: 36px;

  border: none;

  background: #e5aa00;

  cursor: pointer;

  font-size: 12px;
}


.search-button:hover {
  background: #d99f00;
}


.application-name {
  font-size: 13px;

  font-weight: bold;

  color: #333;
}


/* =========================================================
   DASHBOARD
   ========================================================= */

.dashboard {
  width: 100%;

  max-width: 1400px;

  margin: 0 auto;

  padding: 25px;
}


/* =========================================================
   DASHBOARD HEADING
   ========================================================= */

.dashboard-heading {
  display: flex;

  justify-content: space-between;

  align-items: center;

  margin-bottom: 20px;
}


.dashboard h1 {
  margin: 0;

  font-size: 27px;

  color: #333;
}


.subtitle {
  margin: 5px 0 0 0;

  color: #777;

  font-size: 13px;
}


.refresh-main-btn {
  padding: 9px 16px;

  border: none;

  border-radius: 5px;

  background: #1976d2;

  color: white;

  cursor: pointer;

  font-size: 12px;
}


.refresh-main-btn:hover {
  background: #125ea8;
}


/* =========================================================
   ERROR
   ========================================================= */

.error-message {
  padding: 12px 16px;

  margin-bottom: 18px;

  border-radius: 5px;

  background: #ffebee;

  color: #c62828;

  border: 1px solid #ffcdd2;

  font-size: 13px;
}


/* =========================================================
   SUCCESS
   ========================================================= */

.success-message {
  padding: 12px 16px;

  margin-bottom: 18px;

  border-radius: 5px;

  background: #e8f5e9;

  color: #2e7d32;

  border: 1px solid #c8e6c9;

  font-size: 13px;
}


/* =========================================================
   PROJECT SELECTOR
   ========================================================= */

.project-selector {
  display: flex;

  justify-content: space-between;

  align-items: flex-end;

  gap: 25px;

  padding: 15px;

  margin-bottom: 18px;

  background: white;

  border: 1px solid #ddd;

  border-radius: 5px;
}


.selector-left {
  display: flex;

  flex-direction: column;

  gap: 6px;

  width: 45%;
}


.selector-left label {
  font-size: 11px;

  font-weight: bold;

  color: #555;
}


.selector-left select {
  width: 100%;

  padding: 9px;

  border: 1px solid #ddd;

  border-radius: 4px;

  background: white;

  font-size: 12px;

  outline: none;
}


.owner-name-display {
  display: flex;

  flex-direction: column;

  gap: 5px;

  font-size: 11px;

  color: #777;
}


.owner-name-display strong {
  color: #333;

  font-size: 13px;
}


/* =========================================================
   PROJECT CARD
   ========================================================= */

.project-card {
  background: white;

  border: 1px solid #ddd;

  border-radius: 5px;

  padding: 20px;

  margin-bottom: 30px;
}


/* =========================================================
   PROJECT INFORMATION
   ========================================================= */

.project-information {
  display: flex;

  justify-content: space-between;

  align-items: flex-start;

  padding-bottom: 18px;

  border-bottom: 1px solid #e5e5e5;
}


.project-info-left h2 {
  margin: 0 0 10px 0;

  font-size: 20px;

  color: #333;
}


.project-info-left p {
  margin: 5px 0;

  font-size: 12px;

  color: #666;
}


.project-dates {
  display: flex;

  gap: 35px;
}


.project-dates div {
  display: flex;

  flex-direction: column;

  gap: 5px;
}


.project-dates span {
  font-size: 10px;

  color: #888;
}


.project-dates strong {
  font-size: 12px;

  color: #333;
}


/* =========================================================
   PROJECT ACTIONS
   ========================================================= */

.project-actions {
  display: flex;

  gap: 8px;

  margin: 15px 0;
}


.edit-button,
.delete-button {
  padding: 8px 14px;

  border: none;

  border-radius: 4px;

  cursor: pointer;

  font-size: 11px;

  font-weight: 600;
}


.edit-button {
  background: #1976d2;

  color: white;
}


.edit-button:hover {
  background: #125ea8;
}


.delete-button {
  background: #d32f2f;

  color: white;
}


.delete-button:hover {
  background: #b71c1c;
}


/* =========================================================
   STATISTICS
   ========================================================= */

.stats {
  display: grid;

  grid-template-columns:
    repeat(5, 1fr);

  gap: 10px;

  margin: 18px 0;
}


.stat-card {
  padding: 14px;

  text-align: center;

  border: 1px solid #e2e2e2;

  background: #fafafa;

  border-radius: 5px;
}


.stat-number {
  font-size: 23px;

  font-weight: bold;

  color: #1976d2;

  margin-bottom: 3px;
}


.stat-label {
  font-size: 10px;

  color: #777;
}


/* =========================================================
   FILTERS
   ========================================================= */

.filters {
  display: flex;

  gap: 7px;

  flex-wrap: wrap;

  padding: 12px 0;

  margin-bottom: 12px;
}


.filters button {
  padding: 7px 13px;

  border: 1px solid #ccc;

  border-radius: 4px;

  background: white;

  color: #555;

  cursor: pointer;

  font-size: 11px;
}


.filters button:hover {
  background: #f1f1f1;
}


.filters button.active {
  background: #ffc400;

  border-color: #e0a900;

  color: #333;

  font-weight: bold;
}


/* =========================================================
   ISSUES SECTION
   ========================================================= */

.issues-section {
  margin-top: 15px;
}


.issues-heading {
  display: flex;

  justify-content: space-between;

  align-items: center;

  margin-bottom: 12px;
}


.issues-heading h2 {
  margin: 0;

  font-size: 18px;

  color: #333;
}


.issues-heading span {
  font-size: 11px;

  color: #888;
}


/* =========================================================
   KANBAN BOARD
   ========================================================= */

.kanban-board {
  display: grid;

  grid-template-columns:
    repeat(3, minmax(0, 1fr));

  gap: 12px;

  width: 100%;
}


.kanban-column {
  min-width: 0;

  background: #f5f6f7;

  border-radius: 4px;

  padding: 8px;

  min-height: 250px;
}


/* =========================================================
   KANBAN HEADERS
   ========================================================= */

.kanban-column-header {
  display: flex;

  justify-content: space-between;

  align-items: center;

  padding: 9px 10px;

  margin-bottom: 8px;

  background: white;

  border-top: 3px solid;

  border-radius: 3px;

  font-size: 11px;

  font-weight: bold;
}


.open-header {
  border-color: #e53935;
}


.progress-header {
  border-color: #1976d2;
}


.closed-header {
  border-color: #43a047;
}


.column-count {
  display: inline-flex;

  align-items: center;

  justify-content: center;

  min-width: 19px;

  height: 19px;

  padding: 0 5px;

  border-radius: 50%;

  background: #eee;

  font-size: 10px;
}


/* =========================================================
   KANBAN CARDS
   ========================================================= */

.kanban-cards {
  display: flex;

  flex-direction: column;

  gap: 8px;
}


.kanban-issue {
  background: white;

  border: 1px solid #ddd;

  border-radius: 4px;

  padding: 11px;

  box-shadow:
    0 1px 3px rgba(0, 0, 0, 0.06);

  transition:
    transform 0.15s ease,
    box-shadow 0.15s ease;
}


.kanban-issue:hover {
  transform: translateY(-2px);

  box-shadow:
    0 3px 8px rgba(0, 0, 0, 0.10);
}


/* =========================================================
   ISSUE CARD HEADER
   ========================================================= */

.issue-card-header {
  display: flex;

  justify-content: space-between;

  align-items: center;

  gap: 5px;

  margin-bottom: 8px;
}


.issue-number {
  font-size: 9px;

  color: #888;
}


.priority-badge {
  padding: 3px 6px;

  border-radius: 3px;

  font-size: 8px;

  font-weight: bold;
}


.priority-badge.high {
  background: #ffebee;

  color: #c62828;
}


.priority-badge.medium {
  background: #fff8e1;

  color: #f57f17;
}


.priority-badge.low {
  background: #e8f5e9;

  color: #2e7d32;
}


/* =========================================================
   ISSUE TITLE
   ========================================================= */

.kanban-issue h3 {
  margin: 0 0 7px 0;

  font-size: 12px;

  line-height: 1.4;

  color: #333;

  cursor: pointer;
}


.kanban-issue h3:hover {
  color: #1976d2;

  text-decoration: underline;
}


/* =========================================================
   DESCRIPTION
   ========================================================= */

.issue-description {
  margin: 0 0 9px 0;

  font-size: 10px;

  line-height: 1.4;

  color: #777;

  display: -webkit-box;

  -webkit-line-clamp: 3;

  -webkit-box-orient: vertical;

  overflow: hidden;
}


/* =========================================================
   ISSUE META
   ========================================================= */

.issue-meta {
  display: flex;

  justify-content: space-between;

  gap: 5px;

  padding-top: 8px;

  border-top: 1px solid #eee;

  font-size: 9px;

  color: #777;
}


.issue-assignee {
  display: flex;

  justify-content: space-between;

  margin-top: 7px;

  font-size: 9px;

  color: #777;
}


.issue-assignee strong {
  color: #333;
}


/* =========================================================
   CARD CONTROLS
   ========================================================= */

.card-controls {
  display: flex;

  gap: 5px;

  margin-top: 9px;
}


.card-controls select {
  flex: 1;

  min-width: 0;

  padding: 5px;

  border: 1px solid #ddd;

  border-radius: 3px;

  background: white;

  font-size: 8px;

  cursor: pointer;

  outline: none;
}


.card-controls select:focus {
  border-color: #1976d2;
}


/* =========================================================
   NO ISSUES
   ========================================================= */

.no-issues {
  text-align: center;

  padding: 40px 20px;

  color: #777;

  background: #fafafa;

  border: 1px solid #e0e0e0;

  border-radius: 5px;

  font-size: 13px;
}


/* =========================================================
   NO PROJECTS
   ========================================================= */

.no-projects-screen {
  background: white;

  border: 1px solid #ddd;

  border-radius: 5px;

  min-height: 400px;

  display: flex;

  flex-direction: column;

  justify-content: center;

  align-items: center;

  text-align: center;
}


.no-projects-icon {
  font-size: 45px;

  margin-bottom: 15px;
}


.no-projects-screen h2 {
  margin: 0 0 7px 0;

  color: #e53935;

  font-size: 18px;
}


.no-projects-screen p {
  color: #777;

  font-size: 12px;
}


.create-project-empty-btn {
  margin-top: 10px;

  padding: 8px 14px;

  border: 1px solid #ffc400;

  background: #fff;

  color: #b88600;

  border-radius: 4px;

  cursor: pointer;

  font-size: 11px;
}


.create-project-empty-btn:hover {
  background: #ffc400;

  color: #333;
}


/* =========================================================
   EDIT PROJECT
   ========================================================= */

.edit-project-card {
  max-width: 700px;

  margin: 20px auto;

  padding: 25px;

  background: white;

  border-radius: 6px;

  border: 1px solid #ddd;

  box-shadow:
    0 2px 10px rgba(0, 0, 0, 0.08);
}


.edit-project-card h2 {
  margin-top: 0;

  margin-bottom: 25px;

  font-size: 20px;
}


.form-group {
  margin-bottom: 18px;
}


.form-group label {
  display: block;

  margin-bottom: 6px;

  font-size: 12px;

  font-weight: 600;
}


.form-group input {
  width: 100%;

  padding: 10px;

  border: 1px solid #ccc;

  border-radius: 4px;

  font-size: 13px;

  outline: none;
}


.form-group input:focus {
  border-color: #1976d2;
}


.edit-buttons {
  display: flex;

  gap: 10px;

  margin-top: 20px;
}


.save-button,
.cancel-button {
  padding: 9px 16px;

  border: none;

  border-radius: 4px;

  cursor: pointer;

  font-weight: 600;
}


.save-button {
  background: #1976d2;

  color: white;
}


.cancel-button {
  background: #ddd;

  color: #333;
}


/* =========================================================
   RESPONSIVE
   ========================================================= */

@media (max-width: 1100px) {

  .kanban-board {
    grid-template-columns:
      repeat(3, minmax(0, 1fr));
  }

}


@media (max-width: 900px) {

  .sidebar {
    width: 190px;
  }


  .main-content {
    margin-left: 190px;

    width: calc(100% - 190px);
  }


  .stats {
    grid-template-columns:
      repeat(3, 1fr);
  }


  .kanban-board {
    grid-template-columns: 1fr;
  }


  .project-information {
    flex-direction: column;

    gap: 20px;
  }


  .project-dates {
    text-align: left;
  }

}


@media (max-width: 768px) {

  .sidebar {
    position: relative;

    width: 100%;

    min-height: auto;

    padding: 15px;
  }


  .main-content {
    margin-left: 0;

    width: 100%;
  }


  .app-layout {
    flex-direction: column;
  }


  .sidebar-menu {
    display: flex;

    flex-wrap: wrap;

    gap: 5px;
  }


  .sidebar-link {
    width: auto;

    flex: 1;
  }


  .sidebar-logout {
    position: static;

    margin-top: 15px;
  }


  .app-header {
    flex-direction: column;

    height: auto;

    gap: 10px;

    padding: 10px;
  }


  .search-container {
    width: 100%;
  }


  .application-name {
    align-self: flex-end;
  }


  .dashboard {
    padding: 15px;
  }


  .dashboard-heading {
    flex-direction: column;

    align-items: stretch;

    gap: 10px;
  }


  .refresh-main-btn {
    align-self: flex-start;
  }


  .project-selector {
    flex-direction: column;

    align-items: stretch;
  }


  .selector-left {
    width: 100%;
  }


  .stats {
    grid-template-columns:
      1fr 1fr;
  }

}


@media (max-width: 500px) {

  .stats {
    grid-template-columns: 1fr;
  }


  .project-actions {
    flex-direction: column;
  }


  .edit-button,
  .delete-button {
    width: 100%;
  }


  .project-dates {
    flex-direction: column;

    gap: 10px;
  }


  .filters {
    flex-direction: column;
  }


  .filters button {
    width: 100%;
  }


  .sidebar-menu {
    flex-direction: column;
  }


  .sidebar-link {
    width: 100%;
  }

}

<div class="app-layout">

  <!-- =====================================================
       SIDEBAR
       ===================================================== -->

  <aside class="sidebar">

    <!-- PROFILE -->

    <div class="profile-section">

      <img
  class="profile-image"
  [src]="profileImage || 'assets/default-profile.png'"
  [alt]="ownerName + ' Profile'"
  (error)="profileImage = 'assets/default-profile.png'"
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



  <!-- =====================================================
       MAIN CONTENT
       ===================================================== -->

  <main class="main-content">


    <!-- APPLICATION HEADER -->

    <header class="app-header">

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


      <div class="application-name">

        Issue Tracking System

      </div>

    </header>



    <!-- ===================================================
         DASHBOARD
         =================================================== -->

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



      <!-- =================================================
           NO PROJECTS
           ================================================= -->

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



      <!-- =================================================
           PROJECT AREA
           ================================================= -->

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

                <strong>
                  Project ID:
                </strong>

                {{ selectedProject.id }}

              </p>

              <p>

                <strong>
                  Project Owner:
                </strong>

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
              [class.active]="
                selectedFilter === 'ALL'
              "
              (click)="filterIssues('ALL')">

              All

            </button>


            <button
              type="button"
              [class.active]="
                selectedFilter === 'OPEN'
              "
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
              [class.active]="
                selectedFilter === 'HIGH'
              "
              (click)="filterIssues('HIGH')">

              High Priority

            </button>


            <button
              type="button"
              [class.active]="
                selectedFilter === 'MEDIUM'
              "
              (click)="filterIssues('MEDIUM')">

              Medium

            </button>


            <button
              type="button"
              [class.active]="
                selectedFilter === 'CLOSED'
              "
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
              *ngIf="
                filteredIssues.length === 0
              ">

              No issues found for this project.

            </div>



            <!-- =================================================
                 KANBAN BOARD
                 ================================================= -->

            <div
              class="kanban-board"
              *ngIf="
                filteredIssues.length > 0
              ">


              <!-- =================================================
                   TO DO
                   ================================================= -->

              <div class="kanban-column">

                <div
                  class="kanban-column-header open-header">

                  <span>
                    TO DO
                  </span>

                  <span class="column-count">

                    {{
                      getBoardIssues('OPEN').length
                    }}

                  </span>

                </div>


                <div class="kanban-cards">

                  <div
                    class="kanban-issue"
                    *ngFor="
                      let issue of
                      getBoardIssues('OPEN')
                    ">


                    <div class="issue-card-header">

                      <span class="issue-number">

                        ID:
                        {{ issue.id }}

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
                      (click)="
                        openIssueDetails(issue)
                      ">

                      {{ issue.summary }}

                    </h3>


                    <p class="issue-description">

                      {{ issue.description }}

                    </p>


                    <div class="issue-meta">

                      <span>

                        Type:
                        {{ issue.type }}

                      </span>

                      <span>

                        Points:
                        {{ issue.storyPoint }}

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



              <!-- =================================================
                   DEVELOPMENT
                   ================================================= -->

              <div class="kanban-column">

                <div
                  class="kanban-column-header progress-header">

                  <span>
                    DEVELOPMENT
                  </span>

                  <span class="column-count">

                    {{
                      getBoardIssues(
                        'IN_PROGRESS'
                      ).length
                    }}

                  </span>

                </div>


                <div class="kanban-cards">

                  <div
                    class="kanban-issue"
                    *ngFor="
                      let issue of
                      getBoardIssues(
                        'IN_PROGRESS'
                      )
                    ">


                    <div class="issue-card-header">

                      <span class="issue-number">

                        ID:
                        {{ issue.id }}

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
                      (click)="
                        openIssueDetails(issue)
                      ">

                      {{ issue.summary }}

                    </h3>


                    <p class="issue-description">

                      {{ issue.description }}

                    </p>


                    <div class="issue-meta">

                      <span>

                        Type:
                        {{ issue.type }}

                      </span>

                      <span>

                        Points:
                        {{ issue.storyPoint }}

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



              <!-- =================================================
                   COMPLETED
                   ================================================= -->

              <div class="kanban-column">

                <div
                  class="kanban-column-header closed-header">

                  <span>
                    COMPLETED
                  </span>

                  <span class="column-count">

                    {{
                      getBoardIssues('CLOSED').length
                    }}

                  </span>

                </div>


                <div class="kanban-cards">

                  <div
                    class="kanban-issue"
                    *ngFor="
                      let issue of
                      getBoardIssues('CLOSED')
                    ">


                    <div class="issue-card-header">

                      <span class="issue-number">

                        ID:
                        {{ issue.id }}

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
                      (click)="
                        openIssueDetails(issue)
                      ">

                      {{ issue.summary }}

                    </h3>


                    <p class="issue-description">

                      {{ issue.description }}

                    </p>


                    <div class="issue-meta">

                      <span>

                        Type:
                        {{ issue.type }}

                      </span>

                      <span>

                        Points:
                        {{ issue.storyPoint }}

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

    </div>

  </main>

</div>

import {
  Component,
  OnInit
} from '@angular/core';

import {
  CommonModule
} from '@angular/common';

import {
  FormsModule
} from '@angular/forms';

import {
  Router
} from '@angular/router';

import {
  IssueService
} from '../../services/issue.service';

import {
  ProjectService
} from '../../services/project.service';

import {
  Issue
} from '../../models/issue';

import {
  Project
} from '../../models/project';


@Component({
  selector: 'app-owner-dashboard',
  standalone: true,

  imports: [
    CommonModule,
    FormsModule
  ],

  templateUrl:
    './owner-dashboard.component.html',

  styleUrls: [
    './owner-dashboard.component.css'
  ]
})
export class OwnerDashboardComponent
  implements OnInit {


  // =========================================================
  // LOGGED-IN OWNER
  // =========================================================

  ownerId: number = 0;

  ownerName: string =
    'Project Owner';

  ownerEmail: string = '';

  profileImage: string =
    'assets/default-profile.png';


  // =========================================================
  // SEARCH
  // =========================================================

  searchText: string = '';


  // =========================================================
  // PROJECTS
  // =========================================================

  projects: Project[] = [];

  selectedProject:
    Project | null = null;


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

    private projectService:
      ProjectService,

    private issueService:
      IssueService,

    private router:
      Router

  ) {}


  // =========================================================
  // INIT
  // =========================================================

  ngOnInit(): void {

    const storedUser =
      localStorage.getItem('user');


    // -------------------------------------------------------
    // USER NOT LOGGED IN
    // -------------------------------------------------------

    if (!storedUser) {

      this.router.navigate([
        '/login'
      ]);

      return;
    }


    // -------------------------------------------------------
    // READ USER
    // -------------------------------------------------------

    try {

      const user =
        JSON.parse(storedUser);


      this.ownerId =
        Number(user.id);


      this.ownerName =
        user.name ||
        'Project Owner';


      this.ownerEmail =
        user.email ||
        '';


      // -----------------------------------------------------
      // PROFILE IMAGE
      // -----------------------------------------------------

      this.profileImage =
        user.profileImage ||
        user.profileImageUrl ||
        user.profile ||
        user.image ||
        'assets/default-profile.png';


      // -----------------------------------------------------
      // IF ONLY FILE NAME IS STORED
      // Example:
      // profile.jpg
      //
      // It becomes:
      // assets/profile.jpg
      // -----------------------------------------------------

      if (
        this.profileImage &&
        !this.profileImage.startsWith('http') &&
        !this.profileImage.startsWith('https') &&
        !this.profileImage.startsWith('data:image') &&
        !this.profileImage.startsWith('assets/')
      ) {

        this.profileImage =
          'assets/' +
          this.profileImage;
      }


    } catch (error) {

      console.error(
        'Failed to read logged-in user:',
        error
      );


      this.router.navigate([
        '/login'
      ]);

      return;
    }


    // -------------------------------------------------------
    // VALIDATE OWNER
    // -------------------------------------------------------

    if (!this.ownerId) {

      this.errorMessage =
        'Logged-in user information is missing.';

      return;
    }


    // -------------------------------------------------------
    // LOAD PROJECTS
    // -------------------------------------------------------

    this.loadProjects();
  }


  // =========================================================
  // PROFILE IMAGE ERROR
  // =========================================================

  handleImageError(): void {

    this.profileImage =
      'assets/default-profile.png';
  }


  // =========================================================
  // LOAD PROJECTS
  // =========================================================

  loadProjects(): void {

    this.errorMessage = '';


    this.projectService
      .getProjectsByOwner(
        this.ownerId
      )
      .subscribe({

        next: (
          data: Project[]
        ) => {

          this.projects =
            data || [];


          if (
            this.projects.length > 0
          ) {

            // ------------------------------------------------
            // KEEP CURRENT PROJECT SELECTED
            // ------------------------------------------------

            if (
              this.selectedProject &&
              this.selectedProject.id !==
                undefined
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


            // ------------------------------------------------
            // LOAD ISSUES
            // ------------------------------------------------

            this.loadIssues();


          } else {

            this.selectedProject =
              null;

            this.issues = [];
          }

        },


        error: (
          error: any
        ) => {

          console.error(
            'Failed to load projects:',
            error
          );


          this.errorMessage =
            'Failed to load projects.';


          this.projects = [];

          this.selectedProject =
            null;

          this.issues = [];
        }

      });
  }


  // =========================================================
  // SELECT PROJECT
  // =========================================================

  selectProject(
    project: Project
  ): void {

    this.selectedProject =
      project;


    this.selectedFilter =
      'ALL';


    this.searchText =
      '';


    this.issues = [];


    this.errorMessage = '';


    this.editMode =
      false;


    this.loadIssues();
  }


  // =========================================================
  // LOAD ISSUES
  // =========================================================

  loadIssues(): void {

    if (
      !this.selectedProject ||
      this.selectedProject.id ===
        undefined
    ) {

      this.issues = [];

      return;
    }


    const projectId =
      this.selectedProject.id;


    this.issueService
      .getIssuesByProject(
        projectId
      )
      .subscribe({

        next: (
          data: Issue[]
        ) => {

          this.issues =
            data || [];

          this.errorMessage = '';
        },


        error: (
          error: any
        ) => {

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

    this.editMode =
      false;

    this.successMessage =
      '';

    this.searchText =
      '';

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
  // CREATE ISSUE
  // =========================================================

  createIssue(): void {

    this.router.navigate([
      '/create-issue'
    ]);
  }


  // =========================================================
  // LOGOUT
  // =========================================================

  logout(): void {

    localStorage.removeItem(
      'user'
    );

    this.router.navigate([
      '/login'
    ]);
  }


  // =========================================================
  // START EDIT PROJECT
  // =========================================================

  startEditProject(): void {

    if (
      !this.selectedProject
    ) {

      alert(
        'Please select a project.'
      );

      return;
    }


    this.errorMessage = '';

    this.successMessage = '';


    this.editProjectData = {

      id:
        this.selectedProject.id,

      projectName:
        this.selectedProject.projectName,

      productOwnerId:
        this.selectedProject.productOwnerId,

      startDate:
        this.selectedProject.startDate,

      endDate:
        this.selectedProject.endDate

    };


    this.editMode =
      true;
  }


  // =========================================================
  // CANCEL EDIT
  // =========================================================

  cancelEditProject(): void {

    this.editMode =
      false;

    this.errorMessage =
      '';

    this.successMessage =
      '';
  }


  // =========================================================
  // SAVE PROJECT
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

        next: (
          updatedProject: Project
        ) => {

          this.successMessage =
            'Project updated successfully.';


          this.editMode =
            false;


          this.selectedProject =
            updatedProject;


          this.loadProjects();
        },


        error: (
          error: any
        ) => {

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
      this.selectedProject.id ===
        undefined
    ) {

      alert(
        'Please select a project.'
      );

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


    this.errorMessage =
      '';

    this.successMessage =
      '';


    this.projectService
      .deleteProject(
        projectId
      )
      .subscribe({

        next: () => {

          this.successMessage =
            'Project deleted successfully.';


          this.selectedProject =
            null;


          this.issues = [];


          this.editMode =
            false;


          this.loadProjects();
        },


        error: (
          error: any
        ) => {

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

    this.selectedFilter =
      filter;
  }


  // =========================================================
  // SEARCH + FILTERED ISSUES
  // =========================================================

  get filteredIssues(): Issue[] {

    let result =
      this.issues;


    if (
      this.selectedFilter ===
      'OPEN'
    ) {

      result =
        result.filter(
          issue =>
            issue.status ===
            'OPEN'
        );

    } else if (
      this.selectedFilter ===
      'IN_PROGRESS'
    ) {

      result =
        result.filter(
          issue =>
            issue.status ===
            'IN_PROGRESS'
        );

    } else if (
      this.selectedFilter ===
      'HIGH'
    ) {

      result =
        result.filter(
          issue =>
            issue.priority ===
            'HIGH'
        );

    } else if (
      this.selectedFilter ===
      'MEDIUM'
    ) {

      result =
        result.filter(
          issue =>
            issue.priority ===
            'MEDIUM'
        );

    } else if (
      this.selectedFilter ===
      'CLOSED'
    ) {

      result =
        result.filter(
          issue =>
            issue.status ===
            'CLOSED'
        );
    }


    const search =
      this.searchText
        .trim()
        .toLowerCase();


    if (search) {

      result =
        result.filter(
          issue => {

            const summary =
              issue.summary
                ?.toLowerCase() ||
              '';


            const description =
              issue.description
                ?.toLowerCase() ||
              '';


            return (
              summary.includes(search) ||
              description.includes(search)
            );
          }
        );
    }


    return result;
  }


  // =========================================================
  // BOARD ISSUES
  // =========================================================

  getBoardIssues(
    status: string
  ): Issue[] {

    return this.filteredIssues.filter(
      issue =>
        issue.status === status
    );
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
        issue.status ===
        'OPEN'
    ).length;
  }


  getHighPriorityIssues(): number {

    return this.issues.filter(
      issue =>
        issue.priority ===
        'HIGH'
    ).length;
  }


  getInProgressIssues(): number {

    return this.issues.filter(
      issue =>
        issue.status ===
        'IN_PROGRESS'
    ).length;
  }


  getClosedIssues(): number {

    return this.issues.filter(
      issue =>
        issue.status ===
        'CLOSED'
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

      alert(
        'Issue ID is missing.'
      );

      return;
    }


    const select =
      event.target as
      HTMLSelectElement;


    const status =
      select.value;


    this.issueService
      .updateIssueStatus(
        issue.id,
        status
      )
      .subscribe({

        next: (
          updatedIssue: Issue
        ) => {

          issue.status =
            updatedIssue.status;
        },


        error: (
          error: any
        ) => {

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

      alert(
        'Issue ID is missing.'
      );

      return;
    }


    const select =
      event.target as
      HTMLSelectElement;


    const priority =
      select.value;


    this.issueService
      .updateIssuePriority(
        issue.id,
        priority
      )
      .subscribe({

        next: (
          updatedIssue: Issue
        ) => {

          issue.priority =
            updatedIssue.priority;
        },


        error: (
          error: any
        ) => {

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

      alert(
        'Issue ID is missing.'
      );

      return;
    }


    const select =
      event.target as
      HTMLSelectElement;


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

        next: (
          updatedIssue: Issue
        ) => {

          issue.assigneeId =
            updatedIssue.assigneeId;
        },


        error: (
          error: any
        ) => {

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

    if (
      status === 'OPEN'
    ) {

      return 'open';
    }


    if (
      status === 'IN_PROGRESS'
    ) {

      return 'progress';
    }


    if (
      status === 'CLOSED'
    ) {

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

    if (
      priority === 'HIGH'
    ) {

      return 'high';
    }


    if (
      priority === 'MEDIUM'
    ) {

      return 'medium';
    }


    if (
      priority === 'LOW'
    ) {

      return 'low';
    }


    return '';
  }


  // =========================================================
  // ISSUE DETAILS
  // =========================================================

  openIssueDetails(
    issue: Issue
  ): void {

    if (!issue.id) {

      alert(
        'Issue ID is missing.'
      );

      return;
    }


    this.router.navigate([
      '/issue',
      issue.id
    ]);
  }

}
