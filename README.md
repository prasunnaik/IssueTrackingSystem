<div class="dashboard">

  <h1>Project Owner Dashboard</h1>

  <p class="subtitle">
    Manage your projects and track issues
  </p>

  <!-- Error Message -->
  <p *ngIf="errorMessage" class="error">
    {{ errorMessage }}
  </p>

  <!-- Refresh Button -->
  <button class="refresh-btn" (click)="loadProjects()">
    Refresh
  </button>

  <!-- Projects -->
  <div *ngIf="projects.length > 0">

    <div
      class="project-card"
      *ngFor="let project of projects"
    >

      <!-- Project Header -->
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


      <!-- Summary Cards -->
      <div class="summary">

        <div class="summary-card">
          <h3>{{ issues.length }}</h3>
          <span>Total Issues</span>
        </div>

        <div class="summary-card">
          <h3>{{ getOpenIssues() }}</h3>
          <span>Open Issues</span>
        </div>

        <div class="summary-card">
          <h3>{{ getHighPriorityIssues() }}</h3>
          <span>High Priority</span>
        </div>

      </div>


      <!-- Issues -->
      <h3 class="issues-title">
        Issues
      </h3>

      <div
        class="issue-card"
        *ngFor="let issue of issues"
      >

        <!-- Issue Header -->
        <div class="issue-header">

          <h4>
            {{ issue.summary }}
          </h4>

          <span class="status">
            {{ issue.status }}
          </span>

        </div>


        <!-- Issue Information -->
        <p>
          <strong>Issue ID:</strong>
          {{ issue.id }}
        </p>

        <p>
          <strong>Description:</strong>
          {{ issue.description }}
        </p>


        <!-- Issue Details -->
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


        <!-- Update Status -->
        <div class="status-update">

          <label for="status">
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


      <!-- No Issues -->
      <p
        *ngIf="issues.length === 0"
        class="no-issues"
      >
        No issues found for this project.
      </p>

    </div>

  </div>


  <!-- No Projects -->
  <div
    *ngIf="projects.length === 0 && !errorMessage"
    class="no-projects"
  >
    No projects found.
  </div>

</div>



/* Main Dashboard */

.dashboard {
  max-width: 1100px;
  margin: 40px auto;
  padding: 30px;
  font-family: Arial, Helvetica, sans-serif;
  color: #333;
  background-color: #f7f9fc;
  min-height: 100vh;
}


/* Page Heading */

.dashboard h1 {
  text-align: center;
  margin-bottom: 8px;
  font-size: 32px;
  color: #222;
}

.subtitle {
  text-align: center;
  color: #777;
  margin-bottom: 30px;
}


/* Error */

.error {
  background-color: #ffe5e5;
  color: #c62828;
  padding: 12px 16px;
  border-radius: 6px;
  margin-bottom: 20px;
}


/* Refresh Button */

.refresh-btn {
  display: block;
  margin-left: auto;
  margin-bottom: 20px;

  padding: 10px 20px;

  border: none;
  border-radius: 6px;

  background-color: #1976d2;
  color: white;

  font-size: 14px;
  font-weight: 600;

  cursor: pointer;
}

.refresh-btn:hover {
  background-color: #125ea8;
}


/* Project Card */

.project-card {
  background-color: white;

  border: 1px solid #e0e0e0;
  border-radius: 12px;

  padding: 25px;

  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);

  margin-bottom: 30px;
}


/* Project Header */

.project-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;

  padding-bottom: 20px;

  border-bottom: 1px solid #e5e5e5;
}

.project-header h2 {
  margin: 0 0 8px 0;
  font-size: 24px;
  color: #333;
}

.project-header p {
  margin: 5px 0;
  color: #666;
}

.dates {
  text-align: right;
}

.dates p {
  margin: 5px 0;
}


/* Summary */

.summary {
  display: grid;

  grid-template-columns: repeat(3, 1fr);

  gap: 20px;

  margin: 25px 0;
}


.summary-card {
  border: 1px solid #e0e0e0;

  border-radius: 8px;

  padding: 20px;

  text-align: center;

  background-color: #fafafa;
}


.summary-card h3 {
  margin: 0 0 8px 0;

  font-size: 30px;

  color: #1976d2;
}


.summary-card span {
  color: #777;

  font-size: 14px;
}


/* Issues Heading */

.issues-title {
  font-size: 20px;

  margin-top: 25px;
  margin-bottom: 15px;

  color: #333;
}


/* Issue Card */

.issue-card {
  border: 1px solid #e1e1e1;

  border-radius: 8px;

  padding: 20px;

  margin-bottom: 15px;

  background-color: #fff;
}


.issue-card:hover {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
}


/* Issue Header */

.issue-header {
  display: flex;

  justify-content: space-between;

  align-items: center;

  margin-bottom: 15px;
}


.issue-header h4 {
  margin: 0;

  font-size: 18px;

  color: #333;
}


/* Status */

.status {
  padding: 6px 12px;

  border-radius: 20px;

  background-color: #e8f5e9;

  color: #2e7d32;

  font-size: 12px;

  font-weight: bold;
}


/* Issue Information */

.issue-card p {
  margin: 8px 0;

  font-size: 14px;

  color: #555;
}


/* Issue Details */

.issue-details {
  display: flex;

  flex-wrap: wrap;

  gap: 20px;

  margin-top: 15px;

  padding-top: 15px;

  border-top: 1px solid #eeeeee;
}


.issue-details span {
  font-size: 13px;

  color: #555;
}


/* Status Update */

.status-update {
  display: flex;

  align-items: center;

  gap: 12px;

  margin-top: 20px;

  padding-top: 15px;

  border-top: 1px solid #eeeeee;
}


.status-update label {
  font-weight: 600;

  font-size: 14px;
}


.status-update select {
  padding: 8px 12px;

  border: 1px solid #ccc;

  border-radius: 6px;

  background-color: white;

  font-size: 14px;

  cursor: pointer;
}


.status-update select:focus {
  outline: none;

  border-color: #1976d2;
}


/* No Issues */

.no-issues {
  text-align: center;

  padding: 20px;

  color: #777;
}


/* No Projects */

.no-projects {
  text-align: center;

  padding: 40px;

  color: #777;

  background-color: white;

  border-radius: 10px;
}


/* Responsive */

@media (max-width: 768px) {

  .dashboard {
    margin: 10px;
    padding: 15px;
  }

  .project-header {
    flex-direction: column;
  }

  .dates {
    text-align: left;
    margin-top: 15px;
  }

  .summary {
    grid-template-columns: 1fr;
  }

  .issue-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 10px;
  }

  .issue-details {
    flex-direction: column;
    gap: 8px;
  }

}
