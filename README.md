.dashboard {
  min-height: 100vh;
  padding: 30px;
  background: #f5f6fa;
}

.dashboard-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 25px;
}

.dashboard-header h1 {
  margin: 0;
}

.subtitle {
  color: #666;
  margin-top: 5px;
}

.header-actions {
  display: flex;
  gap: 10px;
}

.refresh-button,
.logout-button {
  padding: 10px 18px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.refresh-button {
  background: #1976d2;
  color: white;
}

.logout-button {
  background: #d32f2f;
  color: white;
}

.profile-card {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 20px;
  margin-bottom: 25px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

.profile-avatar {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #1976d2;
  color: white;
  font-size: 22px;
  font-weight: bold;
}

.profile-card h3 {
  margin: 0;
}

.profile-card p {
  margin: 5px 0 0;
  color: #777;
}

.search-section {
  margin-bottom: 25px;
}

.search-section input {
  width: 100%;
  max-width: 600px;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 6px;
  box-sizing: border-box;
}

.stats {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 15px;
  margin-bottom: 30px;
}

.stat-card {
  padding: 20px;
  text-align: center;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

.stat-number {
  font-size: 28px;
  font-weight: bold;
}

.stat-label {
  margin-top: 5px;
  color: #666;
}

.error-message {
  padding: 12px;
  margin-bottom: 20px;
  background: #ffebee;
  color: #c62828;
  border-radius: 5px;
}

.success-message {
  padding: 12px;
  margin-bottom: 20px;
  background: #e8f5e9;
  color: #2e7d32;
  border-radius: 5px;
}

.loading,
.empty-message {
  padding: 30px;
  text-align: center;
  background: white;
  border-radius: 8px;
}

.kanban {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}

.column {
  min-width: 0;
  background: #eee;
  border-radius: 8px;
  padding: 12px;
}

.column-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  margin-bottom: 12px;
  border-radius: 6px;
  background: white;
}

.column-header h2 {
  margin: 0;
  font-size: 18px;
}

.column-header span {
  padding: 4px 9px;
  border-radius: 20px;
  background: #ddd;
  font-weight: bold;
}

.issue-card {
  background: white;
  padding: 16px;
  margin-bottom: 12px;
  border-radius: 7px;
  box-shadow: 0 1px 5px rgba(0, 0, 0, 0.08);
}

.issue-title {
  margin: 0 0 10px;
  cursor: pointer;
  color: #1976d2;
}

.issue-title:hover {
  text-decoration: underline;
}

.issue-description {
  color: #666;
  font-size: 14px;
  line-height: 1.5;
}

.issue-info {
  display: flex;
  flex-direction: column;
  gap: 7px;
  margin-top: 12px;
  font-size: 14px;
}

.high {
  color: #d32f2f;
  font-weight: bold;
}

.medium {
  color: #ef6c00;
  font-weight: bold;
}

.low {
  color: #2e7d32;
  font-weight: bold;
}

.status-control {
  margin-top: 15px;
}

.status-control label {
  display: block;
  margin-bottom: 5px;
  font-weight: 600;
}

.status-control select {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.todo-header {
  border-top: 4px solid #1976d2;
}

.development-header {
  border-top: 4px solid #ef6c00;
}

.testing-header {
  border-top: 4px solid #7b1fa2;
}

.completed-header {
  border-top: 4px solid #2e7d32;
}

@media (max-width: 1000px) {

  .kanban {
    grid-template-columns: repeat(2, 1fr);
  }

  .stats {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (max-width: 600px) {

  .dashboard {
    padding: 15px;
  }

  .dashboard-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 15px;
  }

  .kanban {
    grid-template-columns: 1fr;
  }

  .stats {
    grid-template-columns: repeat(2, 1fr);
  }
}

<div class="dashboard">

  <!-- ===================================================== -->
  <!-- HEADER -->
  <!-- ===================================================== -->

  <div class="dashboard-header">

    <div>

      <h1>
        Assignee Dashboard
      </h1>

      <p class="subtitle">
        View and manage your assigned issues
      </p>

    </div>


    <div class="header-actions">

      <button
        type="button"
        class="refresh-button"
        (click)="refreshDashboard()">

        Refresh

      </button>


      <button
        type="button"
        class="logout-button"
        (click)="logout()">

        Logout

      </button>

    </div>

  </div>


  <!-- ===================================================== -->
  <!-- PROFILE -->
  <!-- ===================================================== -->

  <div class="profile-card">

    <div class="profile-avatar">

      {{ assigneeName.charAt(0) }}

    </div>

    <div>

      <h3>
        {{ assigneeName }}
      </h3>

      <p>
        Assignee ID: {{ assigneeId }}
      </p>

    </div>

  </div>


  <!-- ===================================================== -->
  <!-- ERROR -->
  <!-- ===================================================== -->

  <div
    class="error-message"
    *ngIf="errorMessage">

    {{ errorMessage }}

  </div>


  <!-- ===================================================== -->
  <!-- SUCCESS -->
  <!-- ===================================================== -->

  <div
    class="success-message"
    *ngIf="successMessage">

    {{ successMessage }}

  </div>


  <!-- ===================================================== -->
  <!-- SEARCH -->
  <!-- ===================================================== -->

  <div class="search-section">

    <input
      type="text"
      [(ngModel)]="searchText"
      placeholder="Search by summary or description..."
    />

  </div>


  <!-- ===================================================== -->
  <!-- STATISTICS -->
  <!-- ===================================================== -->

  <div class="stats">

    <div class="stat-card">

      <div class="stat-number">
        {{ getTotalIssues() }}
      </div>

      <div class="stat-label">
        Total Assigned
      </div>

    </div>


    <div class="stat-card">

      <div class="stat-number">
        {{ getTodoCount() }}
      </div>

      <div class="stat-label">
        TO-DO
      </div>

    </div>


    <div class="stat-card">

      <div class="stat-number">
        {{ getDevelopmentCount() }}
      </div>

      <div class="stat-label">
        Development
      </div>

    </div>


    <div class="stat-card">

      <div class="stat-number">
        {{ getTestingCount() }}
      </div>

      <div class="stat-label">
        Testing
      </div>

    </div>


    <div class="stat-card">

      <div class="stat-number">
        {{ getCompletedCount() }}
      </div>

      <div class="stat-label">
        Completed
      </div>

    </div>

  </div>


  <!-- ===================================================== -->
  <!-- LOADING -->
  <!-- ===================================================== -->

  <div
    class="loading"
    *ngIf="loading">

    Loading assigned issues...

  </div>


  <!-- ===================================================== -->
  <!-- NO ISSUES -->
  <!-- ===================================================== -->

  <div
    class="empty-message"
    *ngIf="
      !loading &&
      filteredIssues.length === 0 &&
      !errorMessage
    ">

    No assigned issues found.

  </div>


  <!-- ===================================================== -->
  <!-- KANBAN BOARD -->
  <!-- ===================================================== -->

  <div
    class="kanban"
    *ngIf="
      !loading &&
      filteredIssues.length > 0
    ">


    <!-- ================================================= -->
    <!-- TO-DO -->
    <!-- ================================================= -->

    <div class="column">

      <div class="column-header todo-header">

        <h2>
          TO-DO
        </h2>

        <span>
          {{ getTodoIssues().length }}
        </span>

      </div>


      <div
        class="issue-card"
        *ngFor="let issue of getTodoIssues()"
        (click)="openIssue(issue)">

        <h3 class="issue-title">

          {{ issue.summary }}

        </h3>


        <p class="issue-description">

          {{ issue.description }}

        </p>


        <div class="issue-info">

          <span>

            <strong>Priority:</strong>

            <span
              [ngClass]="
                getPriorityClass(issue.priority)
              ">

              {{ issue.priority }}

            </span>

          </span>


          <span>

            <strong>Type:</strong>

            {{ issue.type }}

          </span>


          <span>

            <strong>Story:</strong>

            {{ issue.storyPoint }}

          </span>

        </div>

      </div>

    </div>


    <!-- ================================================= -->
    <!-- DEVELOPMENT -->
    <!-- ================================================= -->

    <div class="column">

      <div class="column-header development-header">

        <h2>
          Development
        </h2>

        <span>
          {{ getDevelopmentIssues().length }}
        </span>

      </div>


      <div
        class="issue-card"
        *ngFor="let issue of getDevelopmentIssues()"
        (click)="openIssue(issue)">

        <h3 class="issue-title">

          {{ issue.summary }}

        </h3>


        <p class="issue-description">

          {{ issue.description }}

        </p>


        <div class="issue-info">

          <span>

            <strong>Priority:</strong>

            <span
              [ngClass]="
                getPriorityClass(issue.priority)
              ">

              {{ issue.priority }}

            </span>

          </span>


          <span>

            <strong>Type:</strong>

            {{ issue.type }}

          </span>


          <span>

            <strong>Story:</strong>

            {{ issue.storyPoint }}

          </span>

        </div>

      </div>

    </div>


    <!-- ================================================= -->
    <!-- TESTING -->
    <!-- ================================================= -->

    <div class="column">

      <div class="column-header testing-header">

        <h2>
          Testing
        </h2>

        <span>
          {{ getTestingIssues().length }}
        </span>

      </div>


      <div
        class="issue-card"
        *ngFor="let issue of getTestingIssues()"
        (click)="openIssue(issue)">

        <h3 class="issue-title">

          {{ issue.summary }}

        </h3>


        <p class="issue-description">

          {{ issue.description }}

        </p>


        <div class="issue-info">

          <span>

            <strong>Priority:</strong>

            <span
              [ngClass]="
                getPriorityClass(issue.priority)
              ">

              {{ issue.priority }}

            </span>

          </span>


          <span>

            <strong>Type:</strong>

            {{ issue.type }}

          </span>


          <span>

            <strong>Story:</strong>

            {{ issue.storyPoint }}

          </span>

        </div>

      </div>

    </div>


    <!-- ================================================= -->
    <!-- COMPLETED -->
    <!-- ================================================= -->

    <div class="column">

      <div class="column-header completed-header">

        <h2>
          Completed
        </h2>

        <span>
          {{ getCompletedIssues().length }}
        </span>

      </div>


      <div
        class="issue-card"
        *ngFor="let issue of getCompletedIssues()"
        (click)="openIssue(issue)">

        <h3 class="issue-title">

          {{ issue.summary }}

        </h3>


        <p class="issue-description">

          {{ issue.description }}

        </p>


        <div class="issue-info">

          <span>

            <strong>Priority:</strong>

            <span
              [ngClass]="
                getPriorityClass(issue.priority)
              ">

              {{ issue.priority }}

            </span>

          </span>


          <span>

            <strong>Type:</strong>

            {{ issue.type }}

          </span>


          <span>

            <strong>Story:</strong>

            {{ issue.storyPoint }}

          </span>

        </div>

      </div>

    </div>

  </div>

</div>

import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { Router } from '@angular/router';

import { IssueService } from '../../services/issue.service';
import { Issue } from '../../models/issue';

@Component({
  selector: 'app-assignee-dashboard',
  standalone: true,
  imports: [
    CommonModule,
    FormsModule
  ],
  templateUrl: './assignee-dashboard.component.html',
  styleUrl: './assignee-dashboard.component.css'
})
export class AssigneeDashboardComponent implements OnInit {

  // =========================================================
  // ASSIGNEE
  // =========================================================

  // Temporary hardcoded ID.
  // Later this will come from login/authentication.
  assigneeId: number = 0;

  assigneeName: string = 'Assignee';


  // =========================================================
  // ISSUES
  // =========================================================

  issues: Issue[] = [];

  loading: boolean = false;

  errorMessage: string = '';

  successMessage: string = '';


  // =========================================================
  // SEARCH
  // =========================================================

  searchText: string = '';


  // =========================================================
  // CONSTRUCTOR
  // =========================================================

  constructor(
    private issueService: IssueService,
    private router: Router
  ) {}


  // =========================================================
  // INIT
  // =========================================================

  ngOnInit(): void {

  const storedUser = localStorage.getItem('user');

  if (!storedUser) {

    this.router.navigate(['/login']);

    return;
  }

  const user = JSON.parse(storedUser);

  this.assigneeId = Number(user.id);
  this.assigneeName = user.name || 'Assignee';

  if (!this.assigneeId) {

    this.errorMessage =
      'Logged-in user information is missing.';

    return;
  }

  this.loadIssues();
}


  // =========================================================
  // LOAD ASSIGNED ISSUES
  // =========================================================

  loadIssues(): void {

    this.loading = true;
    this.errorMessage = '';

    this.issueService
      .getIssuesByAssignee(this.assigneeId)
      .subscribe({

        next: (data: Issue[]) => {

          this.issues = data || [];

          this.loading = false;
        },

        error: (error: any) => {

          console.error(
            'Failed to load assigned issues:',
            error
          );

          this.issues = [];

          this.loading = false;

          this.errorMessage =
            'Failed to load assigned issues.';
        }
      });
  }


  // =========================================================
  // REFRESH
  // =========================================================

  refreshDashboard(): void {

    this.successMessage = '';

    this.loadIssues();
  }


  // =========================================================
  // SEARCH
  // =========================================================

  get filteredIssues(): Issue[] {

    const search =
      this.searchText
        .trim()
        .toLowerCase();

    if (!search) {
      return this.issues;
    }

    return this.issues.filter(issue =>

      issue.summary
        ?.toLowerCase()
        .includes(search)

      ||

      issue.description
        ?.toLowerCase()
        .includes(search)
    );
  }


  // =========================================================
  // TODO ISSUES
  // =========================================================

  getTodoIssues(): Issue[] {

    return this.filteredIssues.filter(
      issue => issue.status === 'OPEN'
    );
  }


  // =========================================================
  // DEVELOPMENT ISSUES
  // =========================================================

  getDevelopmentIssues(): Issue[] {

    return this.filteredIssues.filter(
      issue => issue.status === 'IN_PROGRESS'
    );
  }


  // =========================================================
  // TESTING ISSUES
  // =========================================================

  getTestingIssues(): Issue[] {

    return this.filteredIssues.filter(
      issue => issue.status === 'TESTING'
    );
  }


  // =========================================================
  // COMPLETED ISSUES
  // =========================================================

  getCompletedIssues(): Issue[] {

    return this.filteredIssues.filter(
      issue => issue.status === 'CLOSED'
    );
  }


  // =========================================================
  // COUNTS
  // =========================================================

  getTotalIssues(): number {

    return this.issues.length;
  }


  getTodoCount(): number {

    return this.issues.filter(
      issue => issue.status === 'OPEN'
    ).length;
  }


  getDevelopmentCount(): number {

    return this.issues.filter(
      issue => issue.status === 'IN_PROGRESS'
    ).length;
  }


  getTestingCount(): number {

    return this.issues.filter(
      issue => issue.status === 'TESTING'
    ).length;
  }


  getCompletedCount(): number {

    return this.issues.filter(
      issue => issue.status === 'CLOSED'
    ).length;
  }


  // =========================================================
  // OPEN ISSUE DETAILS
  // =========================================================

  openIssue(issue: Issue): void {

    if (issue.id === undefined) {

      alert('Issue ID is missing.');

      return;
    }

    this.router.navigate([
      '/assignee-issue',
      issue.id
    ]);
  }


  // =========================================================
  // STATUS CLASS
  // =========================================================

  getStatusClass(status: string): string {

    if (status === 'OPEN') {
      return 'todo';
    }

    if (status === 'IN_PROGRESS') {
      return 'development';
    }

    if (status === 'TESTING') {
      return 'testing';
    }

    if (status === 'CLOSED') {
      return 'completed';
    }

    return '';
  }


  // =========================================================
  // PRIORITY CLASS
  // =========================================================

  getPriorityClass(priority: string): string {

    if (priority === 'HIGH') {
      return 'high';
    }

    if (priority === 'MEDIUM') {
      return 'medium';
    }

    if (priority === 'LOW') {
      return 'low';
    }

    return '';
  }


  // =========================================================
  // LOGOUT
  // =========================================================

  logout(): void {

    this.router.navigate([
      '/login'
    ]);
  }
}
