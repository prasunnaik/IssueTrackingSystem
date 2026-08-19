<div class="app-layout">

  <!-- =====================================================
       SIDEBAR
       ===================================================== -->

  <aside class="sidebar">

    <!-- PROFILE -->

    <div class="sidebar-profile">

      <div class="profile-image-wrapper">

        <!-- REAL IMAGE -->

        <img
          *ngIf="profileImage"
          [src]="profileImage"
          [alt]="assigneeName + ' profile'"
          class="profile-image"
          (error)="onProfileImageError()"
        />

        <!-- FALLBACK INITIAL -->

        <div
          *ngIf="!profileImage"
          class="profile-fallback">

          {{ assigneeName.charAt(0).toUpperCase() }}

        </div>

      </div>


      <h3>
        {{ assigneeName }}
      </h3>


      <p class="profile-email">
        {{ assigneeEmail }}
      </p>

    </div>


    <!-- ISSUE COUNT -->

    <div class="sidebar-stats">

      <div class="sidebar-stat-number">
        {{ getTotalIssues() }}
      </div>

      <div class="sidebar-stat-label">
        Issues Assigned
      </div>

    </div>


    <!-- NAVIGATION -->

    <nav class="sidebar-navigation">

      <button
        type="button"
        class="nav-item active"
        (click)="goToDashboard()">

        <span class="nav-icon">
          ▣
        </span>

        <span>
          Issue Dashboard
        </span>

      </button>

    </nav>


    <!-- LOGOUT -->

    <div class="sidebar-bottom">

      <button
        type="button"
        class="logout-button"
        (click)="logout()">

        <span class="nav-icon">
          ↪
        </span>

        Logout

      </button>

    </div>

  </aside>


  <!-- =====================================================
       MAIN CONTENT
       ===================================================== -->

  <main class="main-content">


    <!-- =====================================================
         TOP HEADER
         ===================================================== -->

    <header class="top-header">

      <div class="search-container">

        <input
          type="text"
          [(ngModel)]="searchText"
          placeholder="Search issue by summary or description"
          class="search-input"
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


    <!-- =====================================================
         CONTENT
         ===================================================== -->

    <section class="content-area">


      <!-- PAGE HEADER -->

      <div class="page-header">

        <div>

          <h1>
            Assignee Dashboard
          </h1>

          <p>
            View and manage your assigned issues
          </p>

        </div>


        <button
          type="button"
          class="refresh-button"
          (click)="refreshDashboard()">

          ↻ Refresh

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
           SUMMARY
           ================================================= -->

      <div class="summary-cards">

        <div class="summary-card">

          <div class="summary-number">
            {{ getTotalIssues() }}
          </div>

          <div class="summary-label">
            Total Assigned
          </div>

        </div>


        <div class="summary-card todo-summary">

          <div class="summary-number">
            {{ getTodoCount() }}
          </div>

          <div class="summary-label">
            To Do
          </div>

        </div>


        <div class="summary-card development-summary">

          <div class="summary-number">
            {{ getDevelopmentCount() }}
          </div>

          <div class="summary-label">
            Development
          </div>

        </div>


        <div class="summary-card testing-summary">

          <div class="summary-number">
            {{ getTestingCount() }}
          </div>

          <div class="summary-label">
            Testing
          </div>

        </div>


        <div class="summary-card completed-summary">

          <div class="summary-number">
            {{ getCompletedCount() }}
          </div>

          <div class="summary-label">
            Completed
          </div>

        </div>

      </div>


      <!-- LOADING -->

      <div
        class="loading"
        *ngIf="loading">

        Loading assigned issues...

      </div>


      <!-- EMPTY -->

      <div
        class="empty-message"
        *ngIf="
          !loading &&
          filteredIssues.length === 0 &&
          !errorMessage
        ">

        <div class="empty-title">
          No Issues Assigned
        </div>

        <p>
          There are currently no issues assigned to you.
        </p>

      </div>


      <!-- =================================================
           KANBAN
           ================================================= -->

      <div
        class="kanban"
        *ngIf="
          !loading &&
          filteredIssues.length > 0
        ">


        <!-- =================================================
             TODO
             ================================================= -->

        <div class="kanban-column">

          <div class="column-header todo-header">

            <div class="column-title">

              <span class="column-icon">
                ☰
              </span>

              <span>
                TO DO
              </span>

            </div>

            <span class="column-count">
              {{ getTodoIssues().length }}
            </span>

          </div>


          <div class="column-content">

            <div
              class="issue-card"
              *ngFor="let issue of getTodoIssues()"
              (click)="openIssue(issue)">

              <div class="issue-card-top">

                <span class="issue-id">
                  ID: {{ issue.id }}
                </span>

                <span class="issue-date">
                  {{ issue.createdDate || '' }}
                </span>

              </div>


              <h3 class="issue-title">
                {{ issue.summary }}
              </h3>


              <p class="issue-description">
                {{ issue.description }}
              </p>


              <div class="issue-footer">

                <!-- ASSIGNEE IMAGE -->

                <div class="issue-assignee">

                  <div class="mini-avatar">

                    <img
                      *ngIf="profileImage"
                      [src]="profileImage"
                      [alt]="assigneeName"
                      (error)="onProfileImageError()"
                    />

                    <span *ngIf="!profileImage">

                      {{ assigneeName.charAt(0).toUpperCase() }}

                    </span>

                  </div>


                  <span>
                    {{ assigneeName }}
                  </span>

                </div>


                <span
                  class="priority-badge"
                  [ngClass]="getPriorityClass(issue.priority)">

                  {{ issue.priority }}

                </span>

              </div>


              <div class="issue-bottom-info">

                <span>
                  <strong>Type:</strong>
                  {{ issue.type }}
                </span>

                <span>
                  <strong>Story:</strong>
                  {{ issue.storyPoint }}
                </span>

              </div>

            </div>


            <div
              class="column-empty"
              *ngIf="getTodoIssues().length === 0">

              No issues

            </div>

          </div>

        </div>


        <!-- =================================================
             DEVELOPMENT
             ================================================= -->

        <div class="kanban-column">

          <div class="column-header development-header">

            <div class="column-title">

              <span class="column-icon">
                ⚙
              </span>

              <span>
                DEVELOPMENT
              </span>

            </div>

            <span class="column-count">
              {{ getDevelopmentIssues().length }}
            </span>

          </div>


          <div class="column-content">

            <div
              class="issue-card"
              *ngFor="let issue of getDevelopmentIssues()"
              (click)="openIssue(issue)">

              <div class="issue-card-top">

                <span class="issue-id">
                  ID: {{ issue.id }}
                </span>

                <span class="issue-date">
                  {{ issue.createdDate || '' }}
                </span>

              </div>


              <h3 class="issue-title">
                {{ issue.summary }}
              </h3>


              <p class="issue-description">
                {{ issue.description }}
              </p>


              <div class="issue-footer">

                <div class="issue-assignee">

                  <div class="mini-avatar">

                    <img
                      *ngIf="profileImage"
                      [src]="profileImage"
                      [alt]="assigneeName"
                      (error)="onProfileImageError()"
                    />

                    <span *ngIf="!profileImage">
                      {{ assigneeName.charAt(0).toUpperCase() }}
                    </span>

                  </div>

                  <span>
                    {{ assigneeName }}
                  </span>

                </div>


                <span
                  class="priority-badge"
                  [ngClass]="getPriorityClass(issue.priority)">

                  {{ issue.priority }}

                </span>

              </div>


              <div class="issue-bottom-info">

                <span>
                  <strong>Type:</strong>
                  {{ issue.type }}
                </span>

                <span>
                  <strong>Story:</strong>
                  {{ issue.storyPoint }}
                </span>

              </div>

            </div>


            <div
              class="column-empty"
              *ngIf="getDevelopmentIssues().length === 0">

              No issues

            </div>

          </div>

        </div>


        <!-- =================================================
             TESTING
             ================================================= -->

        <div class="kanban-column">

          <div class="column-header testing-header">

            <div class="column-title">

              <span class="column-icon">
                ✓
              </span>

              <span>
                TESTING
              </span>

            </div>

            <span class="column-count">
              {{ getTestingIssues().length }}
            </span>

          </div>


          <div class="column-content">

            <div
              class="issue-card"
              *ngFor="let issue of getTestingIssues()"
              (click)="openIssue(issue)">

              <div class="issue-card-top">

                <span class="issue-id">
                  ID: {{ issue.id }}
                </span>

                <span class="issue-date">
                  {{ issue.createdDate || '' }}
                </span>

              </div>


              <h3 class="issue-title">
                {{ issue.summary }}
              </h3>


              <p class="issue-description">
                {{ issue.description }}
              </p>


              <div class="issue-footer">

                <div class="issue-assignee">

                  <div class="mini-avatar">

                    <img
                      *ngIf="profileImage"
                      [src]="profileImage"
                      [alt]="assigneeName"
                      (error)="onProfileImageError()"
                    />

                    <span *ngIf="!profileImage">
                      {{ assigneeName.charAt(0).toUpperCase() }}
                    </span>

                  </div>

                  <span>
                    {{ assigneeName }}
                  </span>

                </div>


                <span
                  class="priority-badge"
                  [ngClass]="getPriorityClass(issue.priority)">

                  {{ issue.priority }}

                </span>

              </div>


              <div class="issue-bottom-info">

                <span>
                  <strong>Type:</strong>
                  {{ issue.type }}
                </span>

                <span>
                  <strong>Story:</strong>
                  {{ issue.storyPoint }}
                </span>

              </div>

            </div>


            <div
              class="column-empty"
              *ngIf="getTestingIssues().length === 0">

              No issues

            </div>

          </div>

        </div>


        <!-- =================================================
             COMPLETED
             ================================================= -->

        <div class="kanban-column">

          <div class="column-header completed-header">

            <div class="column-title">

              <span class="column-icon">
                ✓
              </span>

              <span>
                COMPLETED
              </span>

            </div>

            <span class="column-count">
              {{ getCompletedIssues().length }}
            </span>

          </div>


          <div class="column-content">

            <div
              class="issue-card"
              *ngFor="let issue of getCompletedIssues()"
              (click)="openIssue(issue)">

              <div class="issue-card-top">

                <span class="issue-id">
                  ID: {{ issue.id }}
                </span>

                <span class="issue-date">
                  {{ issue.createdDate || '' }}
                </span>

              </div>


              <h3 class="issue-title">
                {{ issue.summary }}
              </h3>


              <p class="issue-description">
                {{ issue.description }}
              </p>


              <div class="issue-footer">

                <div class="issue-assignee">

                  <div class="mini-avatar">

                    <img
                      *ngIf="profileImage"
                      [src]="profileImage"
                      [alt]="assigneeName"
                      (error)="onProfileImageError()"
                    />

                    <span *ngIf="!profileImage">
                      {{ assigneeName.charAt(0).toUpperCase() }}
                    </span>

                  </div>

                  <span>
                    {{ assigneeName }}
                  </span>

                </div>


                <span
                  class="priority-badge"
                  [ngClass]="getPriorityClass(issue.priority)">

                  {{ issue.priority }}

                </span>

              </div>


              <div class="issue-bottom-info">

                <span>
                  <strong>Type:</strong>
                  {{ issue.type }}
                </span>

                <span>
                  <strong>Story:</strong>
                  {{ issue.storyPoint }}
                </span>

              </div>

            </div>


            <div
              class="column-empty"
              *ngIf="getCompletedIssues().length === 0">

              No issues

            </div>

          </div>

        </div>

      </div>

    </section>

  </main>

</div>
