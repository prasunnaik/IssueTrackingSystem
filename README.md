<div class="dashboard">

  <!-- ===================================== -->
  <!-- PAGE HEADER -->
  <!-- ===================================== -->

  <div class="dashboard-header">

    <div>
      <h1>Project Owner Dashboard</h1>

      <p>
        Manage your projects and track issues
      </p>
    </div>

    <button
      type="button"
      class="refresh-button"
      (click)="refresh()">

      Refresh

    </button>

  </div>


  <!-- ===================================== -->
  <!-- ERROR MESSAGE -->
  <!-- ===================================== -->

  <div
    class="error-message"
    *ngIf="errorMessage">

    {{ errorMessage }}

  </div>


  <!-- ===================================== -->
  <!-- PROJECT CARD -->
  <!-- ===================================== -->

  <div
    class="project-card"
    *ngIf="project">

    <!-- PROJECT HEADER -->

    <div class="project-header">

      <div>

        <h2>
          {{ project.name }}
        </h2>

        <p>
          Project ID:
          {{ project.id }}
        </p>

      </div>

      <div class="project-dates">

        <p>
          <strong>Start:</strong>
          {{ project.startDate }}
        </p>

        <p>
          <strong>End:</strong>
          {{ project.endDate }}
        </p>

      </div>

    </div>


    <!-- ================================= -->
    <!-- STATISTICS -->
    <!-- ================================= -->

    <div class="statistics">

      <!-- TOTAL -->

      <div class="stat-card">

        <h3>
          {{ getTotalIssues() }}
        </h3>

        <p>
          Total Issues
        </p>

      </div>


      <!-- OPEN -->

      <div class="stat-card">

        <h3>
          {{ getOpenIssues() }}
        </h3>

        <p>
          Open Issues
        </p>

      </div>


      <!-- HIGH PRIORITY -->

      <div class="stat-card">

        <h3>
          {{ getHighPriorityIssues() }}
        </h3>

        <p>
          High Priority
        </p>

      </div>


      <!-- IN PROGRESS -->

      <div class="stat-card">

        <h3>
          {{ getInProgressIssues() }}
        </h3>

        <p>
          In Progress
        </p>

      </div>


      <!-- CLOSED -->

      <div class="stat-card">

        <h3>
          {{ getClosedIssues() }}
        </h3>

        <p>
          Closed
        </p>

      </div>

    </div>


    <!-- ================================= -->
    <!-- FILTER BUTTONS -->
    <!-- ================================= -->

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


    <!-- ================================= -->
    <!-- ISSUES -->
    <!-- ================================= -->

    <div class="issues-section">

      <h2>
        Issues
      </h2>


      <!-- ISSUE CARD -->

      <div
        class="issue-card"
        *ngFor="let issue of filteredIssues">


        <!-- ISSUE DETAILS -->

        <div class="issue-details">

          <h3>
            {{ issue.title }}
          </h3>


          <p>
            <strong>Issue ID:</strong>
            {{ issue.id }}
          </p>


          <p>
            <strong>Description:</strong>
            {{ issue.description }}
          </p>


          <div class="issue-info">

            <!-- PRIORITY -->

            <span>

              <strong>Priority:</strong>

              <select
                [ngModel]="issue.priority"
                (ngModelChange)="
                  updatePriority(issue, $event)
                ">

                <option value="LOW">
                  LOW
                </option>

                <option value="MEDIUM">
                  MEDIUM
                </option>

                <option value="HIGH">
                  HIGH
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
                (ngModelChange)="
                  updateAssignee(
                    issue,
                    +$event
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

              {{ issue.storyPoints }}

            </span>

          </div>


          <!-- STATUS -->

          <div class="status-update">

            <strong>
              Update Status:
            </strong>


            <select
              [ngModel]="issue.status"
              (ngModelChange)="
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


        <!-- CURRENT STATUS -->

        <div class="issue-status">

          {{ issue.status }}

        </div>

      </div>


      <!-- NO ISSUES -->

      <div
        class="no-issues"
        *ngIf="filteredIssues.length === 0">

        No issues found for this filter.

      </div>

    </div>

  </div>

</div>
