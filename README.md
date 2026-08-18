<div class="dashboard">

  <h1>Project Owner Dashboard</h1>

  <p class="subtitle">
    Manage your projects and track issues
  </p>

  <p *ngIf="errorMessage" class="error">
    {{ errorMessage }}
  </p>

  <button class="refresh-btn" (click)="loadProjects()">
    Refresh
  </button>

  <div *ngIf="projects.length > 0">

    <div
      class="project-card"
      *ngFor="let project of projects"
    >

      <div class="project-header">

        <div>
          <h2>{{ project.projectName }}</h2>

          <p>
            Project ID: {{ project.id }}
          </p>
        </div>

        <div class="dates">

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


      <!-- SUMMARY -->

      <div class="summary">

        <div class="summary-card">

          <h3>{{ issues.length }}</h3>

          <span>Total Issues</span>

        </div>


        <div class="summary-card">

          <h3>
            {{
              (issues | filterStatus:'OPEN').length
            }}
          </h3>

          <span>Open Issues</span>

        </div>


        <div class="summary-card">

          <h3>
            {{
              (issues | filterPriority:'HIGH').length
            }}
          </h3>

          <span>High Priority</span>

        </div>

      </div>


      <!-- ISSUES -->

      <h3 class="issues-title">
        Issues
      </h3>


      <div
        class="issue-card"
        *ngFor="let issue of issues"
      >

        <div class="issue-header">

          <h4>
            {{ issue.summary }}
          </h4>

          <span class="status">
            {{ issue.status }}
          </span>

        </div>


        <p>
          <strong>Issue ID:</strong>
          {{ issue.id }}
        </p>


        <p>
          <strong>Description:</strong>
          {{ issue.description }}
        </p>


        <div class="issue-details">

          <span>
            <strong>Priority:</strong>
            {{ issue.priority }}
          </span>

          <span>
            <strong>Type:</strong>
            {{ issue.type }}
          </span>

          <span>
            <strong>Assignee:</strong>
            {{ issue.assigneeId }}
          </span>

          <span>
            <strong>Story Points:</strong>
            {{ issue.storyPoint }}
          </span>

        </div>


        <!-- STATUS UPDATE -->

        <div class="status-update">

          <label>
            Update Status:
          </label>

          <select
            [value]="issue.status"
            (change)="updateStatus(issue, $any($event.target).value)"
          >

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


      <p *ngIf="issues.length === 0">
        No issues found for this project.
      </p>

    </div>

  </div>


  <div
    *ngIf="projects.length === 0 && !errorMessage"
    class="no-projects"
  >
    No projects found.
  </div>

</div>
