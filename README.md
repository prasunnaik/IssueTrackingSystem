<div class="issue-details-page">

  <div class="page-header">

    <div>
      <h1>Issue Details</h1>

      <p>
        View complete information about the issue.
      </p>
    </div>

    <button
      type="button"
      class="back-button"
      (click)="goBack()">

      Back to Dashboard

    </button>

  </div>


  <!-- Loading -->

  <div
    class="loading"
    *ngIf="loading">

    Loading issue details...

  </div>


  <!-- Error -->

  <div
    class="error-message"
    *ngIf="errorMessage">

    {{ errorMessage }}

  </div>


  <!-- Issue -->

  <div
    class="issue-card"
    *ngIf="issue && !loading">


    <!-- HEADER -->

    <div class="issue-header">

      <div>

        <h2>
          {{ issue.summary }}
        </h2>

        <p class="issue-id">
          Issue ID: {{ issue.id }}
        </p>

      </div>


      <span
        class="status-badge"
        [ngClass]="getStatusClass(issue.status)">

        {{ issue.status }}

      </span>

    </div>


    <!-- DESCRIPTION -->

    <div class="section">

      <h3>Description</h3>

      <p>
        {{ issue.description }}
      </p>

    </div>


    <!-- DETAILS -->

    <div class="details-grid">

      <div class="detail">

        <strong>Project ID</strong>

        <span>
          {{ issue.projectId }}
        </span>

      </div>


      <div class="detail">

        <strong>Type</strong>

        <span>
          {{ issue.type }}
        </span>

      </div>


      <div class="detail">

        <strong>Priority</strong>

        <span
          class="priority"
          [ngClass]="getPriorityClass(issue.priority)">

          {{ issue.priority }}

        </span>

      </div>


      <div class="detail">

        <strong>Assignee</strong>

        <span>
          {{ issue.assigneeId }}
        </span>

      </div>


      <div class="detail">

        <strong>Sprint</strong>

        <span>
          {{ issue.sprint || 'Not specified' }}
        </span>

      </div>


      <div class="detail">

        <strong>Story Point</strong>

        <span>
          {{ issue.storyPoint }}
        </span>

      </div>


      <div class="detail">

        <strong>Tags</strong>

        <span>
          {{ issue.tags || 'No tags' }}
        </span>

      </div>


      <div class="detail">

        <strong>Created Date</strong>

        <span>
          {{ issue.createdDate || 'Not available' }}
        </span>

      </div>


      <div class="detail">

        <strong>Last Updated</strong>

        <span>
          {{ issue.lastUpdatedDate || 'Not available' }}
        </span>

      </div>

    </div>


    <!-- ACTIONS -->

    <div class="actions">

      <button
        type="button"
        class="edit-button"
        (click)="editIssue()">

        Edit Issue

      </button>


      <button
        type="button"
        class="delete-button"
        (click)="deleteIssue()">

        Delete Issue

      </button>

    </div>

  </div>

</div>
