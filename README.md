<div class="edit-page">

  <div class="page-header">

    <div>
      <h1>Edit Issue</h1>

      <p>
        Update the issue details below.
      </p>
    </div>

    <button
      type="button"
      class="cancel-button"
      (click)="cancel()">

      Cancel

    </button>

  </div>


  <!-- Loading -->

  <div
    class="loading"
    *ngIf="loading">

    Loading issue...

  </div>


  <!-- Error -->

  <div
    class="error-message"
    *ngIf="errorMessage">

    {{ errorMessage }}

  </div>


  <!-- Success -->

  <div
    class="success-message"
    *ngIf="successMessage">

    {{ successMessage }}

  </div>


  <!-- FORM -->

  <form
    *ngIf="!loading"
    #issueForm="ngForm"
    (ngSubmit)="saveIssue()">


    <!-- SUMMARY -->

    <div class="form-group">

      <label>
        Summary *
      </label>

      <input
        type="text"
        name="summary"
        [(ngModel)]="issue.summary"
        required
        minlength="5"
        maxlength="150"
        placeholder="Enter issue summary">

      <small>
        Minimum 5 characters, maximum 150 characters.
      </small>

    </div>


    <!-- PROJECT -->

    <div class="form-group">

      <label>
        Project ID
      </label>

      <input
        type="number"
        name="projectId"
        [(ngModel)]="issue.projectId"
        readonly>

    </div>


    <!-- TYPE -->

    <div class="form-group">

      <label>
        Type *
      </label>

      <select
        name="type"
        [(ngModel)]="issue.type"
        required>

        <option value="BUG">
          BUG
        </option>

        <option value="TASK">
          TASK
        </option>

        <option value="STORY">
          STORY
        </option>

      </select>

    </div>


    <!-- PRIORITY -->

    <div class="form-group">

      <label>
        Priority *
      </label>

      <select
        name="priority"
        [(ngModel)]="issue.priority"
        required>

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

    </div>


    <!-- DESCRIPTION -->

    <div class="form-group">

      <label>
        Description *
      </label>

      <textarea
        name="description"
        [(ngModel)]="issue.description"
        required
        maxlength="500"
        rows="5"
        placeholder="Enter issue description">
      </textarea>

      <small>
        Maximum 500 characters.
      </small>

    </div>


    <!-- ASSIGNEE -->

    <div class="form-group">

      <label>
        Assignee *
      </label>

      <select
        name="assigneeId"
        [(ngModel)]="issue.assigneeId"
        required>

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

    </div>


    <!-- SPRINT -->

    <div class="form-group">

      <label>
        Sprint
      </label>

      <input
        type="text"
        name="sprint"
        [(ngModel)]="issue.sprint"
        placeholder="Enter sprint">

    </div>


    <!-- TAGS -->

    <div class="form-group">

      <label>
        Tags
      </label>

      <input
        type="text"
        name="tags"
        [(ngModel)]="issue.tags"
        maxlength="100"
        placeholder="Enter tags">

    </div>


    <!-- STORY POINT -->

    <div class="form-group">

      <label>
        Story Point *
      </label>

      <input
        type="number"
        name="storyPoint"
        [(ngModel)]="issue.storyPoint"
        required
        min="1"
        step="1">

    </div>


    <!-- STATUS -->

    <div class="form-group">

      <label>
        Status *
      </label>

      <select
        name="status"
        [(ngModel)]="issue.status"
        required>

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

    <div class="form-buttons">

      <button
        type="submit"
        class="save-button"
        [disabled]="saving || !isFormValid()">

        {{ saving ? 'Saving...' : 'Save Changes' }}

      </button>


      <button
        type="button"
        class="reset-button"
        [disabled]="saving"
        (click)="resetForm()">

        Reset

      </button>


      <button
        type="button"
        class="cancel-button"
        [disabled]="saving"
        (click)="cancel()">

        Cancel

      </button>

    </div>

  </form>

</div>
