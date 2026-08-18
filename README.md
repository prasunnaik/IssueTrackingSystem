<div class="dashboard">

  <!-- ===================================================== -->
  <!-- HEADER -->
  <!-- ===================================================== -->

  <div class="dashboard-header">

    <div>
      <h1>Project Owner Dashboard</h1>
      <p>Manage your projects and track issues</p>
    </div>

    <button
      class="refresh-btn"
      (click)="refreshDashboard()">
      Refresh
    </button>

  </div>


  <!-- ===================================================== -->
  <!-- ERROR MESSAGE -->
  <!-- ===================================================== -->

  <div
    class="error-message"
    *ngIf="errorMessage">

    {{ errorMessage }}

  </div>


  <!-- ===================================================== -->
  <!-- PROJECT CARD -->
  <!-- ===================================================== -->

  <div class="project-card">

    <!-- PROJECT HEADER -->

    <div class="project-header">

      <div>

        <h2>
          {{ project.projectName }}
        </h2>

        <p>
          Project ID:
          {{ project.id }}
        </p>

      </div>

      <div class="project-dates">

        <div>
          <strong>Start:</strong>
          {{ project.startDate }}
        </div>

        <div>
          <strong>End:</strong>
          {{ project.endDate }}
        </div>

      </div>

    </div>


    <!-- ================================================= -->
    <!-- STATISTICS -->
    <!-- ================================================= -->

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


    <!-- ================================================= -->
    <!-- FILTER BUTTONS -->
    <!-- ================================================= -->

    <div class="filters">

      <button
        [class.active]="selectedFilter === 'ALL'"
        (click)="filterIssues('ALL')">

        All

      </button>


      <button
        [class.active]="selectedFilter === 'OPEN'"
        (click)="filterIssues('OPEN')">

        Open

      </button>


      <button
        [class.active]="selectedFilter === 'IN_PROGRESS'"
        (click)="filterIssues('IN_PROGRESS')">

        In Progress

      </button>


      <button
        [class.active]="selectedFilter === 'HIGH'"
        (click)="filterIssues('HIGH')">

        High Priority

      </button>


      <button
        [class.active]="selectedFilter === 'MEDIUM'"
        (click)="filterIssues('MEDIUM')">

        Medium

      </button>


      <button
        [class.active]="selectedFilter === 'CLOSED'"
        (click)="filterIssues('CLOSED')">

        Closed

      </button>

    </div>


    <!-- ================================================= -->
    <!-- ISSUES SECTION -->
    <!-- ================================================= -->

    <div class="issues-section">

      <h2>Issues</h2>


      <!-- NO ISSUES -->

      <div
        class="no-issues"
        *ngIf="filteredIssues.length === 0">

        No issues found.

      </div>


      <!-- ================================================= -->
      <!-- ISSUE CARD -->
      <!-- ================================================= -->

      <div
        class="issue-card"
        *ngFor="let issue of filteredIssues">


        <!-- ISSUE TOP -->

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


          <!-- STATUS DISPLAY -->

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


        <!-- ================================================= -->
        <!-- ISSUE DETAILS -->
        <!-- ================================================= -->

        <div class="issue-details">


          <!-- PRIORITY -->

          <span>

            <strong>Priority:</strong>

            <select
              [ngModel]="issue.priority"
              (change)="updatePriority(issue, $event)">

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
              (change)="updateAssignee(issue, $event)">

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


          <!-- STORY POINT -->

          <span>

            <strong>Story Points:</strong>

            {{ issue.storyPoint }}

          </span>

        </div>


        <!-- ================================================= -->
        <!-- UPDATE STATUS -->
        <!-- ================================================= -->

        <div class="update-status">

          <label>
            Update Status:
          </label>

          <select
            [ngModel]="issue.status"
            (change)="updateStatus(issue, $event)">

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
