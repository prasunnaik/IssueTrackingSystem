<div class="dashboard">

  <!-- ===================================================== -->
  <!-- HEADER -->
  <!-- ===================================================== -->

  <div class="dashboard-header">

    <div>

      <h1>
        Assignee Dashboard
      </h1>

      <p class="subtitle">
        View and manage your assigned issues
      </p>

    </div>


    <div class="header-actions">

      <button
        type="button"
        class="refresh-button"
        (click)="refreshDashboard()">

        Refresh

      </button>


      <button
        type="button"
        class="logout-button"
        (click)="logout()">

        Logout

      </button>

    </div>

  </div>


  <!-- ===================================================== -->
  <!-- PROFILE -->
  <!-- ===================================================== -->

  <div class="profile-card">

    <div class="profile-avatar">

      {{ assigneeName.charAt(0) }}

    </div>

    <div>

      <h3>
        {{ assigneeName }}
      </h3>

      <p>
        Assignee ID: {{ assigneeId }}
      </p>

    </div>

  </div>


  <!-- ===================================================== -->
  <!-- ERROR -->
  <!-- ===================================================== -->

  <div
    class="error-message"
    *ngIf="errorMessage">

    {{ errorMessage }}

  </div>


  <!-- ===================================================== -->
  <!-- SUCCESS -->
  <!-- ===================================================== -->

  <div
    class="success-message"
    *ngIf="successMessage">

    {{ successMessage }}

  </div>


  <!-- ===================================================== -->
  <!-- SEARCH -->
  <!-- ===================================================== -->

  <div class="search-section">

    <input
      type="text"
      [(ngModel)]="searchText"
      placeholder="Search by summary or description..."
    />

  </div>


  <!-- ===================================================== -->
  <!-- STATISTICS -->
  <!-- ===================================================== -->

  <div class="stats">

    <div class="stat-card">

      <div class="stat-number">
        {{ getTotalIssues() }}
      </div>

      <div class="stat-label">
        Total Assigned
      </div>

    </div>


    <div class="stat-card">

      <div class="stat-number">
        {{ getTodoCount() }}
      </div>

      <div class="stat-label">
        TO-DO
      </div>

    </div>


    <div class="stat-card">

      <div class="stat-number">
        {{ getDevelopmentCount() }}
      </div>

      <div class="stat-label">
        Development
      </div>

    </div>


    <div class="stat-card">

      <div class="stat-number">
        {{ getTestingCount() }}
      </div>

      <div class="stat-label">
        Testing
      </div>

    </div>


    <div class="stat-card">

      <div class="stat-number">
        {{ getCompletedCount() }}
      </div>

      <div class="stat-label">
        Completed
      </div>

    </div>

  </div>


  <!-- ===================================================== -->
  <!-- LOADING -->
  <!-- ===================================================== -->

  <div
    class="loading"
    *ngIf="loading">

    Loading assigned issues...

  </div>


  <!-- ===================================================== -->
  <!-- NO ISSUES -->
  <!-- ===================================================== -->

  <div
    class="empty-message"
    *ngIf="
      !loading &&
      filteredIssues.length === 0 &&
      !errorMessage
    ">

    No assigned issues found.

  </div>


  <!-- ===================================================== -->
  <!-- KANBAN BOARD -->
  <!-- ===================================================== -->

  <div
    class="kanban"
    *ngIf="
      !loading &&
      filteredIssues.length > 0
    ">


    <!-- ================================================= -->
    <!-- TO-DO -->
    <!-- ================================================= -->

    <div class="column">

      <div class="column-header todo-header">

        <h2>
          TO-DO
        </h2>

        <span>
          {{ getTodoIssues().length }}
        </span>

      </div>


      <div
        class="issue-card"
        *ngFor="
          let issue of getTodoIssues()
        ">

        <div class="issue-top">

          <h3
            (click)="openIssue(issue)"
            class="issue-title">

            {{ issue.summary }}

          </h3>

        </div>


        <p class="issue-description">

          {{ issue.description }}

        </p>


        <div class="issue-info">

          <span>
            <strong>Priority:</strong>

            <span
              [ngClass]="
                getPriorityClass(
                  issue.priority
                )
              ">

              {{ issue.priority }}

            </span>
          </span>


          <span>

            <strong>Type:</strong>

            {{ issue.type }}

          </span>


          <span>

            <strong>Story:</strong>

            {{ issue.storyPoint }}

          </span>

        </div>


        <div class="status-control">

          <label>
            Status
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

            <option value="TESTING">
              TESTING
            </option>

            <option value="CLOSED">
              CLOSED
            </option>

          </select>

        </div>

      </div>

    </div>


    <!-- ================================================= -->
    <!-- DEVELOPMENT -->
    <!-- ================================================= -->

    <div class="column">

      <div class="column-header development-header">

        <h2>
          Development
        </h2>

        <span>
          {{ getDevelopmentIssues().length }}
        </span>

      </div>


      <div
        class="issue-card"
        *ngFor="
          let issue of getDevelopmentIssues()
        ">

        <h3
          class="issue-title"
          (click)="openIssue(issue)">

          {{ issue.summary }}

        </h3>


        <p class="issue-description">

          {{ issue.description }}

        </p>


        <div class="issue-info">

          <span>

            <strong>Priority:</strong>

            <span
              [ngClass]="
                getPriorityClass(
                  issue.priority
                )
              ">

              {{ issue.priority }}

            </span>

          </span>


          <span>

            <strong>Type:</strong>

            {{ issue.type }}

          </span>


          <span>

            <strong>Story:</strong>

            {{ issue.storyPoint }}

          </span>

        </div>


        <div class="status-control">

          <label>
            Status
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

            <option value="TESTING">
              TESTING
            </option>

            <option value="CLOSED">
              CLOSED
            </option>

          </select>

        </div>

      </div>

    </div>


    <!-- ================================================= -->
    <!-- TESTING -->
    <!-- ================================================= -->

    <div class="column">

      <div class="column-header testing-header">

        <h2>
          Testing
        </h2>

        <span>
          {{ getTestingIssues().length }}
        </span>

      </div>


      <div
        class="issue-card"
        *ngFor="
          let issue of getTestingIssues()
        ">

        <h3
          class="issue-title"
          (click)="openIssue(issue)">

          {{ issue.summary }}

        </h3>


        <p class="issue-description">

          {{ issue.description }}

        </p>


        <div class="issue-info">

          <span>

            <strong>Priority:</strong>

            <span
              [ngClass]="
                getPriorityClass(
                  issue.priority
                )
              ">

              {{ issue.priority }}

            </span>

          </span>


          <span>

            <strong>Type:</strong>

            {{ issue.type }}

          </span>


          <span>

            <strong>Story:</strong>

            {{ issue.storyPoint }}

          </span>

        </div>


        <div class="status-control">

          <label>
            Status
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

            <option value="TESTING">
              TESTING
            </option>

            <option value="CLOSED">
              CLOSED
            </option>

          </select>

        </div>

      </div>

    </div>


    <!-- ================================================= -->
    <!-- COMPLETED -->
    <!-- ================================================= -->

    <div class="column">

      <div class="column-header completed-header">

        <h2>
          Completed
        </h2>

        <span>
          {{ getCompletedIssues().length }}
        </span>

      </div>


      <div
        class="issue-card"
        *ngFor="
          let issue of getCompletedIssues()
        ">

        <h3
          class="issue-title"
          (click)="openIssue(issue)">

          {{ issue.summary }}

        </h3>


        <p class="issue-description">

          {{ issue.description }}

        </p>


        <div class="issue-info">

          <span>

            <strong>Priority:</strong>

            <span
              [ngClass]="
                getPriorityClass(
                  issue.priority
                )
              ">

              {{ issue.priority }}

            </span>

          </span>


          <span>

            <strong>Type:</strong>

            {{ issue.type }}

          </span>


          <span>

            <strong>Story:</strong>

            {{ issue.storyPoint }}

          </span>

        </div>


        <div class="status-control">

          <label>
            Status
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

            <option value="TESTING">
              TESTING
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
