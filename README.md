<div class="create-issue-container">

  <div class="issue-card">

    <h2>Create Issue</h2>

    <p class="subtitle">
      Create a new issue for your project
    </p>


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


    <!-- SUMMARY -->

    <div class="form-group">

      <label for="summary">
        Summary
      </label>

      <input
        id="summary"
        type="text"
        [(ngModel)]="issue.summary"
        placeholder="Enter issue summary"
      />

    </div>


    <!-- DESCRIPTION -->

    <div class="form-group">

      <label for="description">
        Description
      </label>

      <textarea
        id="description"
        [(ngModel)]="issue.description"
        placeholder="Enter issue description"
        rows="5">
      </textarea>

    </div>


    <!-- PROJECT -->

    <div class="form-group">

      <label for="project">
        Project
      </label>

      <select
        id="project"
        [(ngModel)]="issue.projectId">

        <option
          [ngValue]="undefined">

          Select Project

        </option>

        <option
          *ngFor="let project of projects"
          [ngValue]="project.id">

          {{ project.projectName }}

        </option>

      </select>

    </div>


    <!-- TYPE -->

    <div class="form-group">

      <label for="type">
        Type
      </label>

      <select
        id="type"
        [(ngModel)]="issue.type">

        <option value="TASK">
          TASK
        </option>

        <option value="BUG">
          BUG
        </option>

        <option value="STORY">
          STORY
        </option>

        <option value="EPIC">
          EPIC
        </option>

      </select>

    </div>


    <!-- PRIORITY -->

    <div class="form-group">

      <label for="priority">
        Priority
      </label>

      <select
        id="priority"
        [(ngModel)]="issue.priority">

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

    </div>


    <!-- STATUS -->

    <div class="form-group">

      <label for="status">
        Status
      </label>

      <select
        id="status"
        [(ngModel)]="issue.status">

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


    <!-- ASSIGNEE -->

    <div class="form-group">

      <label for="assignee">
        Assignee ID
      </label>

      <input
        id="assignee"
        type="number"
        min="1"
        [(ngModel)]="issue.assigneeId"
      />

    </div>


    <!-- STORY POINT -->

    <div class="form-group">

      <label for="storyPoint">
        Story Points
      </label>

      <input
        id="storyPoint"
        type="number"
        min="1"
        [(ngModel)]="issue.storyPoint"
      />

    </div>


    <!-- SPRINT -->

    <div class="form-group">

      <label for="sprint">
        Sprint
      </label>

      <input
        id="sprint"
        type="text"
        [(ngModel)]="issue.sprint"
        placeholder="Enter sprint"
      />

    </div>


    <!-- TAGS -->

    <div class="form-group">

      <label for="tags">
        Tags
      </label>

      <input
        id="tags"
        type="text"
        [(ngModel)]="issue.tags"
        placeholder="Enter tags"
      />

    </div>


    <!-- BUTTONS -->

    <div class="button-container">

      <button
        type="button"
        class="create-button"
        (click)="createIssue()"
        [disabled]="loading">

        {{
          loading
            ? 'Creating...'
            : 'Create Issue'
        }}

      </button>


      <button
        type="button"
        class="back-button"
        (click)="goBack()">

        Back

      </button>

    </div>

  </div>

</div>
