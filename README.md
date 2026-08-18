<div class="dashboard">

  <h1>Project Owner Dashboard</h1>

  <!-- Error -->
  <p *ngIf="errorMessage" class="error">
    {{ errorMessage }}
  </p>

  <!-- Projects -->
  <div *ngIf="projects.length > 0">

    <div class="project-card" *ngFor="let project of projects">

      <h2>{{ project.projectName }}</h2>

      <p>
        <strong>Project ID:</strong>
        {{ project.id }}
      </p>

      <p>
        <strong>Start Date:</strong>
        {{ project.startDate }}
      </p>

      <p>
        <strong>End Date:</strong>
        {{ project.endDate }}
      </p>

      <hr>

      <h3>Issues</h3>

      <!-- Issues -->
      <div *ngIf="issues && issues.length > 0">

        <div class="issue-card" *ngFor="let issue of issues">

          <h4>{{ issue.summary }}</h4>

          <p>
            <strong>Issue ID:</strong>
            {{ issue.id }}
          </p>

          <p>
            <strong>Description:</strong>
            {{ issue.description }}
          </p>

          <p>
            <strong>Status:</strong>
            {{ issue.status }}
          </p>

          <p>
            <strong>Priority:</strong>
            {{ issue.priority }}
          </p>

          <p>
            <strong>Type:</strong>
            {{ issue.type }}
          </p>

          <p>
            <strong>Assignee ID:</strong>
            {{ issue.assigneeId }}
          </p>

          <p>
            <strong>Story Points:</strong>
            {{ issue.storyPoint }}
          </p>

        </div>

      </div>

      <p *ngIf="!issues || issues.length === 0">
        No issues found for this project.
      </p>

    </div>

  </div>

  <!-- No projects -->
  <div *ngIf="projects.length === 0 && !errorMessage">
    No projects found.
  </div>

</div>
