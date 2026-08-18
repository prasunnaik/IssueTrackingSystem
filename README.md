<div class="dashboard">
  <h1>Project Owner Dashboard</h1>

  <p *ngIf="errorMessage" class="error">
    {{ errorMessage }}
  </p>

  <div *ngIf="projects.length === 0 && !errorMessage">
    No projects found.
  </div>

  <div class="project-list" *ngIf="projects.length > 0">
    <div class="project-card" *ngFor="let project of projects">

      <h2>{{ project.projectName }}</h2>

      <p><strong>Project ID:</strong> {{ project.id }}</p>
      <p><strong>Start Date:</strong> {{ project.startDate }}</p>
      <p><strong>End Date:</strong> {{ project.endDate }}</p>

    </div>
  </div>
</div>
