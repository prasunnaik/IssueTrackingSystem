import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';

import { ProjectService } from '../../services/project.service';
import { IssueService } from '../../services/issue.service';

@Component({
  selector: 'app-owner-dashboard',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './owner-dashboard.component.html',
  styleUrls: ['./owner-dashboard.component.css']
})
export class OwnerDashboardComponent implements OnInit {

  projects: any[] = [];
  issues: any[] = [];
  filteredIssues: any[] = [];

  errorMessage = '';

  selectedFilter = 'ALL';

  // Change this if your logged-in owner ID is different
  ownerId = 1;

  constructor(
    private projectService: ProjectService,
    private issueService: IssueService
  ) {}

  ngOnInit(): void {
    this.loadProjects();
  }

  // =========================
  // LOAD PROJECTS
  // =========================
  loadProjects(): void {

    this.projectService.getProjectsByOwner(this.ownerId)
      .subscribe({
        next: (response: any[]) => {

          this.projects = response || [];

          if (this.projects.length === 0) {
            this.errorMessage = 'No projects found.';
            return;
          }

          this.errorMessage = '';

          // Load issues for all projects
          this.loadIssues();
        },

        error: (error) => {
          console.error('Error loading projects:', error);
          this.errorMessage = 'Unable to load projects.';
        }
      });
  }

  // =========================
  // LOAD ISSUES
  // =========================
  loadIssues(): void {

    this.issues = [];

    let completedRequests = 0;

    this.projects.forEach((project: any) => {

      this.issueService
        .getIssuesByProject(project.id)
        .subscribe({

          next: (response: any[]) => {

            if (response && response.length > 0) {
              this.issues.push(...response);
            }

            completedRequests++;

            if (completedRequests === this.projects.length) {
              this.applyFilter();
            }
          },

          error: (error) => {

            console.error(
              `Error loading issues for project ${project.id}:`,
              error
            );

            completedRequests++;

            if (completedRequests === this.projects.length) {
              this.applyFilter();
            }
          }

        });

    });
  }

  // =========================
  // FILTER ISSUES
  // =========================
  filterIssues(filter: string): void {

    this.selectedFilter = filter;

    this.applyFilter();
  }

  applyFilter(): void {

    if (this.selectedFilter === 'ALL') {

      this.filteredIssues = [...this.issues];

    } else if (this.selectedFilter === 'OPEN') {

      this.filteredIssues = this.issues.filter(
        issue => this.normalize(issue.status) === 'OPEN'
      );

    } else if (this.selectedFilter === 'IN_PROGRESS') {

      this.filteredIssues = this.issues.filter(
        issue => this.normalize(issue.status) === 'IN_PROGRESS'
      );

    } else if (this.selectedFilter === 'CLOSED') {

      this.filteredIssues = this.issues.filter(
        issue => this.normalize(issue.status) === 'CLOSED'
      );

    } else if (this.selectedFilter === 'HIGH') {

      this.filteredIssues = this.issues.filter(
        issue => this.normalize(issue.priority) === 'HIGH'
      );

    } else if (this.selectedFilter === 'MEDIUM') {

      this.filteredIssues = this.issues.filter(
        issue => this.normalize(issue.priority) === 'MEDIUM'
      );

    } else {

      this.filteredIssues = [...this.issues];
    }
  }

  // =========================
  // UPDATE ISSUE STATUS
  // =========================
  updateStatus(issue: any, status: string): void {

    this.issueService
      .updateIssueStatus(issue.id, status)
      .subscribe({

        next: (response) => {

          // Update screen immediately
          issue.status = status;

          // Recalculate filtered list
          this.applyFilter();

          console.log('Status updated successfully:', response);
        },

        error: (error) => {

          console.error(
            'Failed to update issue status:',
            error
          );

          alert('Failed to update issue status.');
        }

      });
  }

  // =========================
  // DASHBOARD COUNTS
  // =========================

  getTotalIssues(): number {
    return this.issues.length;
  }

  getOpenIssues(): number {
    return this.issues.filter(
      issue => this.normalize(issue.status) === 'OPEN'
    ).length;
  }

  getHighPriorityIssues(): number {
    return this.issues.filter(
      issue => this.normalize(issue.priority) === 'HIGH'
    ).length;
  }

  getInProgressIssues(): number {
    return this.issues.filter(
      issue => this.normalize(issue.status) === 'IN_PROGRESS'
    ).length;
  }

  getClosedIssues(): number {
    return this.issues.filter(
      issue => this.normalize(issue.status) === 'CLOSED'
    ).length;
  }

  // =========================
  // HELPER
  // =========================
  normalize(value: any): string {

    if (value === null || value === undefined) {
      return '';
    }

    return String(value)
      .trim()
      .toUpperCase()
      .replace(/[\s-]+/g, '_');
  }

  // =========================
  // REFRESH
  // =========================
  refreshDashboard(): void {

    this.errorMessage = '';

    this.loadProjects();
  }
}




<div class="dashboard">

  <!-- ========================= -->
  <!-- HEADER -->
  <!-- ========================= -->

  <div class="dashboard-header">

    <div>
      <h1>Project Owner Dashboard</h1>

      <p class="subtitle">
        Manage your projects and track issues
      </p>
    </div>

    <button
      class="refresh-btn"
      (click)="refreshDashboard()">

      Refresh

    </button>

  </div>


  <!-- ========================= -->
  <!-- ERROR -->
  <!-- ========================= -->

  <div
    *ngIf="errorMessage"
    class="error-message">

    {{ errorMessage }}

  </div>


  <!-- ========================= -->
  <!-- NO PROJECTS -->
  <!-- ========================= -->

  <div
    *ngIf="projects.length === 0 && !errorMessage"
    class="empty-message">

    No projects found.

  </div>


  <!-- ========================= -->
  <!-- PROJECTS -->
  <!-- ========================= -->

  <div
    *ngFor="let project of projects"
    class="project-card">


    <!-- PROJECT HEADER -->

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


    <!-- ========================= -->
    <!-- STATISTICS -->
    <!-- ========================= -->

    <div class="stats-container">


      <div class="stat-card">

        <h3>
          {{ getTotalIssues() }}
        </h3>

        <p>
          Total Issues
        </p>

      </div>


      <div class="stat-card">

        <h3>
          {{ getOpenIssues() }}
        </h3>

        <p>
          Open Issues
        </p>

      </div>


      <div class="stat-card">

        <h3>
          {{ getHighPriorityIssues() }}
        </h3>

        <p>
          High Priority
        </p>

      </div>


      <div class="stat-card">

        <h3>
          {{ getInProgressIssues() }}
        </h3>

        <p>
          In Progress
        </p>

      </div>


      <div class="stat-card">

        <h3>
          {{ getClosedIssues() }}
        </h3>

        <p>
          Closed
        </p>

      </div>

    </div>


    <!-- ========================= -->
    <!-- FILTER BUTTONS -->
    <!-- ========================= -->

    <div class="filter-container">

      <button
        [class.active]="selectedFilter === 'ALL'"
        (click)="filterIssues('ALL')">

        All

      </button>


      <button
        [class.active]="selectedFilter === 'OPEN'"
        (click)="filterIssues('OPEN')">

        Open

      </button>


      <button
        [class.active]="selectedFilter === 'IN_PROGRESS'"
        (click)="filterIssues('IN_PROGRESS')">

        In Progress

      </button>


      <button
        [class.active]="selectedFilter === 'HIGH'"
        (click)="filterIssues('HIGH')">

        High Priority

      </button>


      <button
        [class.active]="selectedFilter === 'MEDIUM'"
        (click)="filterIssues('MEDIUM')">

        Medium

      </button>


      <button
        [class.active]="selectedFilter === 'CLOSED'"
        (click)="filterIssues('CLOSED')">

        Closed

      </button>

    </div>


    <!-- ========================= -->
    <!-- ISSUES -->
    <!-- ========================= -->

    <div class="issues-section">

      <h3>
        Issues
      </h3>


      <!-- NO ISSUES -->

      <div
        *ngIf="filteredIssues.length === 0"
        class="no-issues">

        No issues found for this filter.

      </div>


      <!-- ISSUE CARDS -->

      <div
        *ngFor="let issue of filteredIssues"
        class="issue-card">


        <!-- ISSUE TOP -->

        <div class="issue-header">

          <h4>
            {{ issue.summary }}
          </h4>


          <span
            class="status-badge"
            [class.open]="normalize(issue.status) === 'OPEN'"
            [class.progress]="normalize(issue.status) === 'IN_PROGRESS'"
            [class.closed]="normalize(issue.status) === 'CLOSED'">

            {{ issue.status }}

          </span>

        </div>


        <!-- ISSUE ID -->

        <p>

          <strong>Issue ID:</strong>

          {{ issue.id }}

        </p>


        <!-- DESCRIPTION -->

        <p>

          <strong>Description:</strong>

          {{ issue.description }}

        </p>


        <!-- ISSUE DETAILS -->

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


        <!-- ========================= -->
        <!-- UPDATE STATUS -->
        <!-- ========================= -->

        <div class="update-status">

          <label>
            Update Status:
          </label>


          <select
            [value]="issue.status"
            (change)="updateStatus(
              issue,
              $any($event.target).value
            )">


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

    </div>

  </div>

</div>
