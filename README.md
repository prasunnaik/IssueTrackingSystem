import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';

import { ProjectService } from '../../services/project.service';
import { IssueService } from '../../services/issue.service';

import { Project } from '../../models/project';
import { Issue } from '../../models/issue';

@Component({
  selector: 'app-owner-dashboard',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './owner-dashboard.component.html',
  styleUrls: ['./owner-dashboard.component.css']
})
export class OwnerDashboardComponent implements OnInit {

  projects: Project[] = [];

  issues: Issue[] = [];
  filteredIssues: Issue[] = [];

  errorMessage = '';

  selectedFilter = 'ALL';

  constructor(
    private projectService: ProjectService,
    private issueService: IssueService
  ) {}

  ngOnInit(): void {

    const userData = localStorage.getItem('user');

    if (!userData) {
      this.errorMessage = 'User not logged in';
      return;
    }

    let user: any;

    try {
      user = JSON.parse(userData);
    } catch (error) {
      this.errorMessage = 'Invalid user data';
      return;
    }

    if (!user.id) {
      this.errorMessage = 'User ID not found';
      return;
    }

    // Load projects belonging to the logged-in owner
    this.projectService.getProjectsByOwner(user.id).subscribe({
      next: (response: Project[]) => {

        this.projects = response;

        console.log('Owner projects:', response);

        if (this.projects.length === 0) {
          this.errorMessage = 'No projects found';
          return;
        }

        // Load issues for the first project
        // This matches the current dashboard implementation.
        const projectId = this.projects[0].id;

        if (projectId === undefined) {
          this.errorMessage = 'Project ID not found';
          return;
        }

        this.loadIssues(projectId);
      },

      error: (error) => {

        console.error('Error loading projects:', error);

        this.errorMessage = 'Unable to load projects';
      }
    });
  }


  loadIssues(projectId: number): void {

    this.issueService.getIssuesByProject(projectId).subscribe({

      next: (response: Issue[]) => {

        this.issues = response;

        this.filteredIssues = response;

        console.log('Project issues:', response);
      },

      error: (error) => {

        console.error('Error loading issues:', error);

        this.errorMessage = 'Unable to load issues';
      }
    });
  }


  filterIssues(filter: string): void {

    this.selectedFilter = filter;

    if (filter === 'ALL') {

      this.filteredIssues = this.issues;

      return;
    }

    if (filter === 'OPEN') {

      this.filteredIssues = this.issues.filter(
        issue => issue.status?.toUpperCase() === 'OPEN'
      );

      return;
    }

    if (filter === 'HIGH') {

      this.filteredIssues = this.issues.filter(
        issue => issue.priority?.toUpperCase() === 'HIGH'
      );

      return;
    }

    if (filter === 'MEDIUM') {

      this.filteredIssues = this.issues.filter(
        issue => issue.priority?.toUpperCase() === 'MEDIUM'
      );

      return;
    }

    if (filter === 'CLOSED') {

      this.filteredIssues = this.issues.filter(
        issue => issue.status?.toUpperCase() === 'CLOSED'
      );

      return;
    }

    this.filteredIssues = this.issues;
  }


  get totalIssues(): number {
    return this.issues.length;
  }


  get openIssues(): number {

    return this.issues.filter(
      issue => issue.status?.toUpperCase() === 'OPEN'
    ).length;
  }


  get highPriorityIssues(): number {

    return this.issues.filter(
      issue => issue.priority?.toUpperCase() === 'HIGH'
    ).length;
  }


  refresh(): void {

    window.location.reload();
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
      (click)="refresh()">
      Refresh
    </button>

  </div>


  <!-- Error -->
  <p
    *ngIf="errorMessage"
    class="error">
    {{ errorMessage }}
  </p>


  <!-- Projects -->
  <div
    *ngIf="projects.length > 0"
    class="project-list">

    <div
      class="project-card"
      *ngFor="let project of projects">


      <!-- Project Header -->
      <div class="project-header">

        <div>

          <h2>
            {{ project.projectName }}
          </h2>

          <p>
            Project ID: {{ project.id }}
          </p>

        </div>


        <div class="project-dates">

          <span>
            <strong>Start:</strong>
            {{ project.startDate }}
          </span>

          <span>
            <strong>End:</strong>
            {{ project.endDate }}
          </span>

        </div>

      </div>


      <!-- Statistics -->
      <div class="statistics">


        <!-- Total -->
        <div
          class="stat-card"
          [class.active]="selectedFilter === 'ALL'"
          (click)="filterIssues('ALL')">

          <div class="stat-number">
            {{ totalIssues }}
          </div>

          <div class="stat-label">
            Total Issues
          </div>

        </div>


        <!-- Open -->
        <div
          class="stat-card"
          [class.active]="selectedFilter === 'OPEN'"
          (click)="filterIssues('OPEN')">

          <div class="stat-number">
            {{ openIssues }}
          </div>

          <div class="stat-label">
            Open Issues
          </div>

        </div>


        <!-- High Priority -->
        <div
          class="stat-card"
          [class.active]="selectedFilter === 'HIGH'"
          (click)="filterIssues('HIGH')">

          <div class="stat-number">
            {{ highPriorityIssues }}
          </div>

          <div class="stat-label">
            High Priority
          </div>

        </div>

      </div>


      <!-- Filters -->
      <div class="filters">

        <button
          [class.selected]="selectedFilter === 'ALL'"
          (click)="filterIssues('ALL')">
          All
        </button>

        <button
          [class.selected]="selectedFilter === 'OPEN'"
          (click)="filterIssues('OPEN')">
          Open
        </button>

        <button
          [class.selected]="selectedFilter === 'HIGH'"
          (click)="filterIssues('HIGH')">
          High Priority
        </button>

        <button
          [class.selected]="selectedFilter === 'MEDIUM'"
          (click)="filterIssues('MEDIUM')">
          Medium
        </button>

        <button
          [class.selected]="selectedFilter === 'CLOSED'"
          (click)="filterIssues('CLOSED')">
          Closed
        </button>

      </div>


      <!-- Issues -->
      <div class="issues-section">

        <h3>
          Issues
        </h3>


        <!-- Issues exist -->
        <div
          *ngIf="filteredIssues.length > 0">

          <div
            class="issue-card"
            *ngFor="let issue of filteredIssues">


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


            <!-- Issue details -->
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


        <!-- No issues after filtering -->
        <div
          *ngIf="filteredIssues.length === 0"
          class="no-issues">

          No issues found for this filter.

        </div>

      </div>

    </div>

  </div>


  <!-- No projects -->
  <div
    *ngIf="projects.length === 0 && !errorMessage"
    class="no-projects">

    No projects found.

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
}

.subtitle {
  margin-top: 8px;
  color: #777;
}


/* Refresh */

.refresh-btn {
  padding: 10px 20px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: bold;
}


/* Error */

.error {
  padding: 12px;
  border-radius: 6px;
  margin-bottom: 20px;
}


/* Project */

.project-card {
  border: 1px solid #ddd;
  border-radius: 12px;
  padding: 25px;
  margin-bottom: 30px;
}


.project-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  padding-bottom: 20px;
  border-bottom: 1px solid #ddd;
}


.project-header h2 {
  margin: 0 0 10px 0;
}


.project-header p {
  margin: 0;
}


.project-dates {
  display: flex;
  flex-direction: column;
  gap: 8px;
  text-align: right;
}


/* Statistics */

.statistics {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  margin: 25px 0;
}


.stat-card {
  border: 1px solid #ddd;
  border-radius: 10px;
  padding: 20px;
  text-align: center;
  cursor: pointer;
  transition: 0.2s;
}


.stat-card:hover {
  transform: translateY(-2px);
}


.stat-card.active {
  border: 2px solid #777;
}


.stat-number {
  font-size: 30px;
  font-weight: bold;
}


.stat-label {
  margin-top: 8px;
  color: #777;
}


/* Filters */

.filters {
  display: flex;
  gap: 10px;
  margin-bottom: 25px;
  flex-wrap: wrap;
}


.filters button {
  padding: 9px 16px;
  border: 1px solid #ccc;
  border-radius: 6px;
  background: white;
  cursor: pointer;
}


.filters button.selected {
  font-weight: bold;
  border: 2px solid #555;
}


/* Issues */

.issues-section h3 {
  margin-bottom: 15px;
}


.issue-card {
  border: 1px solid #ddd;
  border-radius: 10px;
  padding: 20px;
  margin-bottom: 15px;
}


.issue-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}


.issue-header h4 {
  margin: 0;
  font-size: 18px;
}


.status {
  padding: 5px 12px;
  border-radius: 15px;
  font-size: 12px;
  font-weight: bold;
}


.issue-details {
  display: flex;
  gap: 25px;
  flex-wrap: wrap;
  margin-top: 15px;
}


.issue-details span {
  font-size: 14px;
}


/* Empty states */

.no-issues,
.no-projects {
  padding: 20px;
  text-align: center;
}


/* Mobile */

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
