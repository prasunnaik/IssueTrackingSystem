<div class="page">

  <!-- HEADER -->

  <div class="header">

    <div>
      <h1>Issue Details</h1>

      <p>
        View and update the assigned issue
      </p>
    </div>

    <button
      type="button"
      class="back-button"
      (click)="goBack()">

      Back to Dashboard

    </button>

  </div>


  <!-- LOADING -->

  <div
    class="loading"
    *ngIf="loading">

    Loading issue details...

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


  <!-- ISSUE DETAILS -->

  <div
    class="issue-container"
    *ngIf="issue && !loading">


    <!-- TITLE -->

    <div class="issue-header">

      <div>

        <div class="breadcrumb">
          Assignee Dashboard / Issue Details
        </div>

        <h2>
          {{ issue.summary }}
        </h2>

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

    <div class="description-section">

      <h3>Description</h3>

      <p>
        {{ issue.description }}
      </p>

    </div>


    <!-- ISSUE INFORMATION -->

    <div class="details-grid">


      <div class="detail-item">

        <span class="label">
          Issue ID
        </span>

        <span class="value">
          {{ issue.id }}
        </span>

      </div>


      <div class="detail-item">

        <span class="label">
          Type
        </span>

        <span class="value">
          {{ issue.type }}
        </span>

      </div>


      <div class="detail-item">

        <span class="label">
          Priority
        </span>

        <span
          class="priority"
          [ngClass]="
            getPriorityClass(issue.priority)
          ">

          {{ issue.priority }}

        </span>

      </div>


      <div class="detail-item">

        <span class="label">
          Assignee ID
        </span>

        <span class="value">
          {{ issue.assigneeId }}
        </span>

      </div>


      <div class="detail-item">

        <span class="label">
          Project ID
        </span>

        <span class="value">
          {{ issue.projectId }}
        </span>

      </div>


      <div class="detail-item">

        <span class="label">
          Story Points
        </span>

        <span class="value">
          {{ issue.storyPoint }}
        </span>

      </div>


      <div class="detail-item">

        <span class="label">
          Sprint
        </span>

        <span class="value">
          {{ issue.sprint || '-' }}
        </span>

      </div>


      <div class="detail-item">

        <span class="label">
          Tags
        </span>

        <span class="value">
          {{ issue.tags || '-' }}
        </span>

      </div>


      <div class="detail-item">

        <span class="label">
          Created Date
        </span>

        <span class="value">
          {{ issue.createdDate || '-' }}
        </span>

      </div>


      <div class="detail-item">

        <span class="label">
          Last Updated
        </span>

        <span class="value">
          {{ issue.lastUpdatedDate || '-' }}
        </span>

      </div>

    </div>


    <!-- STATUS UPDATE -->

    <div class="status-update">

      <label for="status">
        Update Status
      </label>

      <select
        id="status"
        [(ngModel)]="selectedStatus"
        (ngModelChange)="onStatusChange()">

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


    <!-- BUTTONS -->

    <div class="actions">

      <button
        type="button"
        class="save-button"
        [disabled]="
          !hasStatusChanged() || saving
        "
        (click)="saveUpdates()">

        {{ saving ? 'Saving...' : 'Save Updates' }}

      </button>


      <button
        type="button"
        class="cancel-button"
        (click)="goBack()">

        Back

      </button>

    </div>

  </div>

</div>
