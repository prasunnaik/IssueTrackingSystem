import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';

import { ProjectService } from '../../services/project.service';
import { Project } from '../../models/project';
import { IssueService } from '../../services/issue.service';

@Component({
  selector: 'app-owner-dashboard',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './owner-dashboard.component.html',
  styleUrl: './owner-dashboard.component.css'
})
export class OwnerDashboardComponent implements OnInit {

  projects: Project[] = [];

  issues: any[] = [];

  errorMessage = '';

  constructor(
    private projectService: ProjectService,
    private issueService: IssueService
  ) {}

  ngOnInit(): void {
    this.loadDashboard();
  }

  loadDashboard(): void {

    this.errorMessage = '';

    const userData = localStorage.getItem('user');

    if (!userData) {
      this.errorMessage = 'User not logged in';
      return;
    }

    const user = JSON.parse(userData);

    this.projectService.getProjectsByOwner(user.id).subscribe({

      next: (response: Project[]) => {

        this.projects = response;

        console.log('Owner projects:', response);

        this.loadIssues();
      },

      error: (error) => {

        console.error('Error loading projects:', error);

        this.errorMessage = 'Unable to load projects';
      }

    });
  }

  loadIssues(): void {

    this.issues = [];

    if (this.projects.length === 0) {
      return;
    }

    this.projects.forEach((project) => {

      if (project.id !== undefined) {

        this.issueService
          .getIssuesByProject(project.id)
          .subscribe({

            next: (response) => {

              this.issues = [
                ...this.issues,
                ...response
              ];

              console.log(
                'Project issues:',
                project.id,
                response
              );
            },

            error: (error) => {

              console.error(
                'Error loading issues for project:',
                project.id,
                error
              );
            }

          });
      }

    });
  }

  getOpenIssues(): number {

    return this.issues.filter(
      issue =>
        issue.status &&
        issue.status.toString().toUpperCase() === 'OPEN'
    ).length;
  }

  getHighPriorityIssues(): number {

    return this.issues.filter(
      issue =>
        issue.priority &&
        issue.priority.toString().toUpperCase() === 'HIGH'
    ).length;
  }
}


<div class="dashboard">

  <!-- Header -->
  <div class="dashboard-header">

    <div>
      <h1>Project Owner Dashboard</h1>
      <p class="subtitle">
        Manage your projects and track issues
      </p>
    </div>

    <button
      class="refresh-btn"
      (click)="loadDashboard()">
      Refresh
    </button>

  </div>


  <!-- Error Message -->
  <p
    *ngIf="errorMessage"
    class="error">
    {{ errorMessage }}
  </p>


  <!-- No Projects -->
  <div
    *ngIf="projects.length === 0 && !errorMessage"
    class="empty-state">

    <h3>No projects found</h3>

    <p>
      No projects are currently assigned to you.
    </p>

  </div>


  <!-- Projects -->
  <div
    class="project-list"
    *ngIf="projects.length > 0">

    <div
      class="project-card"
      *ngFor="let project of projects">


      <!-- Project Header -->
      <div class="project-header">

        <div>

          <h2>
            {{ project.projectName }}
          </h2>

          <p class="project-id">
            Project ID: {{ project.id }}
          </p>

        </div>

        <div class="project-dates">

          <div>
            <span>Start:</span>
            {{ project.startDate }}
          </div>

          <div>
            <span>End:</span>
            {{ project.endDate }}
          </div>

        </div>

      </div>


      <!-- Statistics -->
      <div class="statistics">

        <div class="stat-card">

          <div class="stat-number">
            {{ issues.length }}
          </div>

          <div class="stat-label">
            Total Issues
          </div>

        </div>


        <div class="stat-card">

          <div class="stat-number">
            {{ getOpenIssues() }}
          </div>

          <div class="stat-label">
            Open Issues
          </div>

        </div>


        <div class="stat-card">

          <div class="stat-number">
            {{ getHighPriorityIssues() }}
          </div>

          <div class="stat-label">
            High Priority
          </div>

        </div>

      </div>


      <!-- Issues -->
      <div class="issues-section">

        <h3>
          Issues
        </h3>


        <!-- Issues Found -->
        <div
          *ngIf="issues.length > 0">

          <div
            class="issue-card"
            *ngFor="let issue of issues">


            <!-- Issue Header -->
            <div class="issue-header">

              <h4>
                {{ issue.summary }}
              </h4>

              <span class="status-badge">
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

          </div>

        </div>


        <!-- No Issues -->
        <p
          *ngIf="issues.length === 0"
          class="no-issues">

          No issues found for this project.

        </p>

      </div>

    </div>

  </div>

</div>


.dashboard {
  max-width: 1100px;
  margin: 0 auto;
  padding: 30px;
  font-family: Arial, sans-serif;
}


/* Header */

.dashboard-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 25px;
}

.dashboard-header h1 {
  margin: 0;
  font-size: 32px;
  color: #222;
}

.subtitle {
  margin-top: 8px;
  color: #777;
}


/* Refresh Button */

.refresh-btn {
  padding: 11px 22px;
  border: none;
  border-radius: 6px;
  background: #1976d2;
  color: white;
  font-size: 14px;
  cursor: pointer;
}

.refresh-btn:hover {
  background: #125ca1;
}


/* Error */

.error {
  padding: 12px;
  border-radius: 6px;
  background: #ffebee;
  color: #c62828;
  margin-bottom: 20px;
}


/* Empty State */

.empty-state {
  text-align: center;
  padding: 40px;
  border: 1px solid #ddd;
  border-radius: 10px;
  color: #777;
}


/* Project */

.project-list {
  display: flex;
  flex-direction: column;
  gap: 25px;
}

.project-card {
  background: white;
  border: 1px solid #ddd;
  border-radius: 12px;
  padding: 25px;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.08);
}


/* Project Header */

.project-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  border-bottom: 1px solid #ddd;
  padding-bottom: 20px;
}

.project-header h2 {
  margin: 0 0 8px 0;
  font-size: 24px;
}

.project-id {
  margin: 0;
  color: #777;
}

.project-dates {
  text-align: right;
  color: #555;
  line-height: 1.8;
}

.project-dates span {
  font-weight: bold;
}


/* Statistics */

.statistics {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 15px;
  margin: 25px 0;
}

.stat-card {
  text-align: center;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background: #fafafa;
}

.stat-number {
  font-size: 30px;
  font-weight: bold;
  color: #1976d2;
}

.stat-label {
  margin-top: 6px;
  color: #777;
}


/* Issues */

.issues-section h3 {
  margin-bottom: 15px;
  font-size: 20px;
}


/* Issue Card */

.issue-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 18px;
  margin-bottom: 15px;
  background: #fff;
}

.issue-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}

.issue-header h4 {
  margin: 0;
  font-size: 18px;
}


/* Status */

.status-badge {
  padding: 6px 12px;
  border-radius: 20px;
  background: #e8f5e9;
  color: #2e7d32;
  font-size: 12px;
  font-weight: bold;
}


/* Issue Details */

.issue-card p {
  margin: 8px 0;
  color: #555;
}

.issue-details {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  margin-top: 15px;
  padding-top: 12px;
  border-top: 1px solid #eee;
}

.issue-details span {
  color: #555;
}


/* No Issues */

.no-issues {
  color: #777;
  padding: 15px;
}


/* Responsive */

@media (max-width: 700px) {

  .dashboard {
    padding: 15px;
  }

  .dashboard-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 15px;
  }

  .project-header {
    flex-direction: column;
    gap: 15px;
  }

  .project-dates {
    text-align: left;
  }

  .statistics {
    grid-template-columns: 1fr;
  }

  .issue-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 10px;
  }

}
