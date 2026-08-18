import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';

import { ProjectsService } from '../../services/project.service';
import { Project } from '../../models/project';
import { IssueService } from '../../services/issue.service';
import { Issue } from '../../models/issue';

@Component({
  selector: 'app-owner-dashboard',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './owner-dashboard.component.html',
  styleUrl: './owner-dashboard.component.css'
})
export class OwnerDashboardComponent implements OnInit {

  projects: Project[] = [];
  issues: Issue[] = [];

  errorMessage = '';

  constructor(
    private projectService: ProjectsService,
    private issueService: IssueService
  ) {}

  ngOnInit(): void {

    const userData = localStorage.getItem('user');

    if (!userData) {
      this.errorMessage = 'User not logged in';
      return;
    }

    const user = JSON.parse(userData);

    this.projectService.getProjectsByOwner(user.id).subscribe({
      next: (response) => {

        this.projects = response;

        console.log('Owner projects:', response);

        // Get issues for the owner's projects
        if (this.projects.length > 0) {

          const projectId = this.projects[0].id;

          if (projectId !== undefined) {

            this.issueService.getIssuesByProject(projectId).subscribe({
              next: (issues) => {

                this.issues = issues;

                console.log('Project issues:', issues);

              },

              error: (error) => {

                console.error('Error loading issues:', error);

                this.errorMessage = 'Unable to load issues';

              }
            });

          }

        }

      },

      error: (error) => {

        console.error('Error loading projects:', error);

        this.errorMessage = 'Unable to load projects';

      }
    });
  }


  getOpenIssues(): number {

    return this.issues.filter(
      issue => issue.status?.toUpperCase() === 'OPEN'
    ).length;

  }


  getHighPriorityIssues(): number {

    return this.issues.filter(
      issue => issue.priority?.toUpperCase() === 'HIGH'
    ).length;

  }

}



<div class="dashboard">

  <h1>Project Owner Dashboard</h1>

  <!-- Error message -->
  <p *ngIf="errorMessage" class="error">
    {{ errorMessage }}
  </p>


  <!-- Projects -->
  <div *ngIf="projects.length > 0">

    <div
      class="project-card"
      *ngFor="let project of projects"
    >

      <!-- Project Header -->
      <div class="project-header">

        <div>

          <h2>
            {{ project.projectName }}
          </h2>

          <p>
            <strong>Project ID:</strong>
            {{ project.id }}
          </p>

        </div>


        <div class="project-dates">

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


      <hr>


      <!-- Issue Summary -->
      <div class="issue-summary">

        <div class="summary-card">

          <h3>
            {{ issues.length }}
          </h3>

          <p>
            Total Issues
          </p>

        </div>


        <div class="summary-card">

          <h3>
            {{ getOpenIssues() }}
          </h3>

          <p>
            Open Issues
          </p>

        </div>


        <div class="summary-card">

          <h3>
            {{ getHighPriorityIssues() }}
          </h3>

          <p>
            High Priority
          </p>

        </div>

      </div>


      <!-- Issues -->
      <h3 class="issues-title">
        Issues
      </h3>


      <div
        *ngIf="issues && issues.length > 0"
      >

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

            <p>
              <strong>Priority:</strong>
              {{ issue.priority }}
            </p>


            <p>
              <strong>Type:</strong>
              {{ issue.type }}
            </p>


            <p>
              <strong>Assignee:</strong>
              {{ issue.assigneeId }}
            </p>


            <p>
              <strong>Story Points:</strong>
              {{ issue.storyPoint }}
            </p>

          </div>

        </div>

      </div>


      <!-- No Issues -->
      <p
        *ngIf="!issues || issues.length === 0"
        class="no-data"
      >
        No issues found for this project.
      </p>

    </div>

  </div>


  <!-- No Projects -->
  <div
    *ngIf="projects.length === 0 && !errorMessage"
    class="no-data"
  >
    No projects found.
  </div>

</div>


.dashboard {
  max-width: 1100px;
  margin: 30px auto;
  padding: 30px;
  font-family: Arial, sans-serif;
  background: #f5f7fa;
  min-height: 100vh;
}


/* Main heading */

.dashboard h1 {
  text-align: center;
  margin-bottom: 30px;
  color: #222;
}


/* Project card */

.project-card {
  background: #ffffff;
  border: 1px solid #ddd;
  border-radius: 12px;
  padding: 25px;
  margin-bottom: 30px;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.08);
}


/* Project header */

.project-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 20px;
}


.project-header h2 {
  margin-top: 0;
  margin-bottom: 15px;
  color: #222;
}


.project-dates {
  text-align: right;
}


.project-dates p {
  margin: 5px 0;
}


/* Horizontal line */

.project-card hr {
  border: 0;
  border-top: 1px solid #ddd;
  margin: 25px 0;
}


/* Issue summary */

.issue-summary {
  display: flex;
  gap: 20px;
  margin: 20px 0 30px;
}


.summary-card {
  flex: 1;
  text-align: center;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 10px;
  background: #f8f9fa;
}


.summary-card h3 {
  font-size: 30px;
  margin: 0 0 8px;
  color: #222;
}


.summary-card p {
  margin: 0;
  color: #666;
}


/* Issues heading */

.issues-title {
  margin-top: 20px;
  margin-bottom: 15px;
  color: #333;
}


/* Issue card */

.issue-card {
  border: 1px solid #ddd;
  border-radius: 10px;
  padding: 20px;
  margin-top: 15px;
  background: #ffffff;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
}


/* Issue header */

.issue-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 15px;
}


.issue-header h4 {
  margin: 0 0 15px;
  font-size: 20px;
  color: #222;
}


/* Status */

.status {
  padding: 6px 14px;
  border-radius: 20px;
  background: #e8f5e9;
  font-size: 13px;
  font-weight: bold;
}


/* Issue details */

.issue-details {
  display: flex;
  gap: 30px;
  flex-wrap: wrap;
  margin-top: 15px;
}


.issue-details p {
  margin: 5px 0;
}


/* Error */

.error {
  color: #b00020;
  background: #ffe5e5;
  border: 1px solid #ffb3b3;
  padding: 12px;
  border-radius: 6px;
}


/* No data */

.no-data {
  padding: 15px;
  color: #666;
  text-align: center;
}


/* Responsive */

@media (max-width: 700px) {

  .dashboard {
    padding: 15px;
    margin: 10px;
  }


  .project-header {
    flex-direction: column;
  }


  .project-dates {
    text-align: left;
  }


  .issue-summary {
    flex-direction: column;
  }


  .issue-header {
    flex-direction: column;
    align-items: flex-start;
  }


  .issue-details {
    flex-direction: column;
    gap: 5px;
  }

}
