import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { Router } from '@angular/router';

import { ProjectService } from '../../services/project.service';
import { Project } from '../../models/project';

@Component({
  selector: 'app-create-project',
  standalone: true,
  imports: [
    CommonModule,
    FormsModule
  ],
  templateUrl: './create-project.component.html',
  styleUrls: ['./create-project.component.css']
})
export class CreateProjectComponent {

  project: Project = {
    projectName: '',
    productOwnerId: 1,
    startDate: '',
    endDate: ''
  };

  errorMessage: string = '';
  successMessage: string = '';
  loading: boolean = false;

  constructor(
    private projectService: ProjectService,
    private router: Router
  ) {}

  createProject(): void {

    this.errorMessage = '';
    this.successMessage = '';

    // Validation
    if (!this.project.projectName.trim()) {
      this.errorMessage = 'Project name is required.';
      return;
    }

    if (!this.project.productOwnerId) {
      this.errorMessage = 'Product owner ID is required.';
      return;
    }

    if (!this.project.startDate) {
      this.errorMessage = 'Start date is required.';
      return;
    }

    if (!this.project.endDate) {
      this.errorMessage = 'End date is required.';
      return;
    }

    if (this.project.endDate < this.project.startDate) {
      this.errorMessage =
        'End date cannot be before start date.';
      return;
    }

    this.loading = true;

    this.projectService
      .createProject(this.project)
      .subscribe({

        next: (createdProject: Project) => {

          this.loading = false;

          this.successMessage =
            'Project created successfully.';

          console.log(
            'Created project:',
            createdProject
          );

          // Clear form
          this.project = {
            projectName: '',
            productOwnerId: 1,
            startDate: '',
            endDate: ''
          };
        },

        error: (error: any) => {

          this.loading = false;

          console.error(
            'Failed to create project:',
            error
          );

          this.errorMessage =
            'Failed to create project.';
        }
      });
  }

  goBack(): void {
    this.router.navigate(['/owner-dashboard']);
  }
}





<div class="create-project-container">

  <div class="project-card">

    <h2>Create Project</h2>

    <p class="subtitle">
      Create a new project
    </p>

    <!-- Error -->
    <div
      class="error-message"
      *ngIf="errorMessage"
    >
      {{ errorMessage }}
    </div>

    <!-- Success -->
    <div
      class="success-message"
      *ngIf="successMessage"
    >
      {{ successMessage }}
    </div>

    <!-- Project Name -->
    <div class="form-group">

      <label for="projectName">
        Project Name
      </label>

      <input
        id="projectName"
        type="text"
        [(ngModel)]="project.projectName"
        placeholder="Enter project name"
      />

    </div>

    <!-- Product Owner -->
    <div class="form-group">

      <label for="productOwnerId">
        Product Owner ID
      </label>

      <input
        id="productOwnerId"
        type="number"
        [(ngModel)]="project.productOwnerId"
        min="1"
      />

    </div>

    <!-- Start Date -->
    <div class="form-group">

      <label for="startDate">
        Start Date
      </label>

      <input
        id="startDate"
        type="date"
        [(ngModel)]="project.startDate"
      />

    </div>

    <!-- End Date -->
    <div class="form-group">

      <label for="endDate">
        End Date
      </label>

      <input
        id="endDate"
        type="date"
        [(ngModel)]="project.endDate"
      />

    </div>

    <!-- Buttons -->
    <div class="button-container">

      <button
        type="button"
        class="create-button"
        (click)="createProject()"
        [disabled]="loading"
      >
        {{ loading ? 'Creating...' : 'Create Project' }}
      </button>

      <button
        type="button"
        class="back-button"
        (click)="goBack()"
      >
        Back
      </button>

    </div>

  </div>

</div>





.create-project-container {
  min-height: 100vh;
  padding: 40px;
  background: #f5f6fa;
}

.project-card {
  max-width: 600px;
  margin: 0 auto;
  padding: 30px;
  background: white;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.project-card h2 {
  margin-bottom: 5px;
  color: #333;
}

.subtitle {
  margin-bottom: 25px;
  color: #777;
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 7px;
  font-weight: 600;
  color: #333;
}

.form-group input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 14px;
  box-sizing: border-box;
}

.form-group input:focus {
  outline: none;
  border-color: #1976d2;
}

.button-container {
  display: flex;
  gap: 10px;
  margin-top: 25px;
}

.create-button,
.back-button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.create-button {
  background: #1976d2;
  color: white;
}

.create-button:hover {
  background: #125aa0;
}

.create-button:disabled {
  background: #aaa;
  cursor: not-allowed;
}

.back-button {
  background: #ddd;
  color: #333;
}

.back-button:hover {
  background: #ccc;
}

.error-message {
  padding: 10px;
  margin-bottom: 20px;
  background: #ffe5e5;
  color: #c62828;
  border-radius: 5px;
}

.success-message {
  padding: 10px;
  margin-bottom: 20px;
  background: #e5f7e5;
  color: #2e7d32;
  border-radius: 5px;
}
