<div class="app-layout">

  <!-- =========================================================
       LEFT SIDEBAR
       ========================================================= -->

  <aside class="sidebar">

    <!-- PROFILE -->

    <div class="sidebar-profile">

      <div class="profile-image-wrapper">

        <img
          *ngIf="profileImage"
          [src]="profileImage"
          [alt]="assigneeName"
          class="profile-image"
        />

        <div
          *ngIf="!profileImage"
          class="profile-fallback">

          {{ assigneeName.charAt(0).toUpperCase() }}

        </div>

      </div>


      <h3>
        {{ assigneeName }}
      </h3>

      <p class="profile-email">
        {{ assigneeEmail }}
      </p>

    </div>


    <!-- ISSUE COUNT -->

    <div class="sidebar-stats">

      <div class="sidebar-stat-number">
        {{ getTotalIssues() }}
      </div>

      <div class="sidebar-stat-label">
        Issues Assigned
      </div>

    </div>


    <!-- NAVIGATION -->

    <nav class="sidebar-navigation">

      <button
        type="button"
        class="nav-item active"
        (click)="goToDashboard()">

        <span class="nav-icon">▣</span>

        <span>
          Issue Dashboard
        </span>

      </button>

    </nav>


    <!-- LOGOUT -->

    <div class="sidebar-bottom">

      <button
        type="button"
        class="logout-button"
        (click)="logout()">

        <span class="nav-icon">↪</span>

        Logout

      </button>

    </div>

  </aside>


  <!-- =========================================================
       MAIN CONTENT
       ========================================================= -->

  <main class="main-content">


    <!-- =======================================================
         APPLICATION HEADER
         ======================================================= -->

    <header class="top-header">

      <!-- SEARCH -->

      <div class="search-container">

        <input
          type="text"
          [(ngModel)]="searchText"
          placeholder="Search issue by summary or description"
          class="search-input"
        />

        <button
          type="button"
          class="search-button">

          🔍

        </button>

      </div>


      <!-- APPLICATION NAME -->

      <div class="application-name">

        Issue Tracking System

      </div>

    </header>


    <!-- =======================================================
         PAGE CONTENT
         ======================================================= -->

    <section class="content-area">


      <!-- PAGE TITLE -->

      <div class="page-header">

        <div>

          <h1>
            Assignee Dashboard
          </h1>

          <p>
            View and manage your assigned issues
          </p>

        </div>


        <button
          type="button"
          class="refresh-button"
          (click)="refreshDashboard()">

          ↻ Refresh

        </button>

      </div>


      <!-- =====================================================
           ERROR
           ===================================================== -->

      <div
        class="error-message"
        *ngIf="errorMessage">

        {{ errorMessage }}

      </div>


      <!-- =====================================================
           SUCCESS
           ===================================================== -->

      <div
        class="success-message"
        *ngIf="successMessage">

        {{ successMessage }}

      </div>


      <!-- =====================================================
           SUMMARY CARDS
           ===================================================== -->

      <div class="summary-cards">

        <div class="summary-card">

          <div class="summary-number">
            {{ getTotalIssues() }}
          </div>

          <div class="summary-label">
            Total Assigned
          </div>

        </div>


        <div class="summary-card todo-summary">

          <div class="summary-number">
            {{ getTodoCount() }}
          </div>

          <div class="summary-label">
            To Do
          </div>

        </div>


        <div class="summary-card development-summary">

          <div class="summary-number">
            {{ getDevelopmentCount() }}
          </div>

          <div class="summary-label">
            Development
          </div>

        </div>


        <div class="summary-card testing-summary">

          <div class="summary-number">
            {{ getTestingCount() }}
          </div>

          <div class="summary-label">
            Testing
          </div>

        </div>


        <div class="summary-card completed-summary">

          <div class="summary-number">
            {{ getCompletedCount() }}
          </div>

          <div class="summary-label">
            Completed
          </div>

        </div>

      </div>


      <!-- =====================================================
           LOADING
           ===================================================== -->

      <div
        class="loading"
        *ngIf="loading">

        Loading assigned issues...

      </div>


      <!-- =====================================================
           NO ISSUES
           ===================================================== -->

      <div
        class="empty-message"
        *ngIf="
          !loading &&
          filteredIssues.length === 0 &&
          !errorMessage
        ">

        <div class="empty-title">
          No Issues Assigned
        </div>

        <p>
          There are currently no issues assigned to you.
        </p>

      </div>


      <!-- =====================================================
           KANBAN BOARD
           ===================================================== -->

      <div
        class="kanban"
        *ngIf="
          !loading &&
          filteredIssues.length > 0
        ">


        <!-- ===================================================
             TO DO
             =================================================== -->

        <div class="kanban-column">

          <div class="column-header todo-header">

            <div class="column-title">

              <span class="column-icon">
                ☰
              </span>

              <span>
                TO DO
              </span>

            </div>

            <span class="column-count">
              {{ getTodoIssues().length }}
            </span>

          </div>


          <div class="column-content">

            <div
              class="issue-card"
              *ngFor="let issue of getTodoIssues()"
              (click)="openIssue(issue)">


              <!-- ISSUE ID -->

              <div class="issue-card-top">

                <span class="issue-id">
                  Id: {{ issue.id }}
                </span>

                <span class="issue-date">
                  {{ issue.createdDate || '' }}
                </span>

              </div>


              <!-- TITLE -->

              <h3 class="issue-title">

                {{ issue.summary }}

              </h3>


              <!-- DESCRIPTION -->

              <p class="issue-description">

                {{ issue.description }}

              </p>


              <!-- ISSUE FOOTER -->

              <div class="issue-footer">

                <div class="issue-assignee">

                  <div class="mini-avatar">

                    <img
                      *ngIf="profileImage"
                      [src]="profileImage"
                      [alt]="assigneeName"
                    />

                    <span *ngIf="!profileImage">
                      {{ assigneeName.charAt(0).toUpperCase() }}
                    </span>

                  </div>

                  <span>
                    {{ assigneeName }}
                  </span>

                </div>


                <span
                  class="priority-badge"
                  [ngClass]="getPriorityClass(issue.priority)">

                  {{ issue.priority }}

                </span>

              </div>


              <!-- STORY POINT -->

              <div class="issue-bottom-info">

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


            <div
              class="column-empty"
              *ngIf="getTodoIssues().length === 0">

              No issues

            </div>

          </div>

        </div>


        <!-- ===================================================
             DEVELOPMENT
             =================================================== -->

        <div class="kanban-column">

          <div class="column-header development-header">

            <div class="column-title">

              <span class="column-icon">
                ⚙
              </span>

              <span>
                DEVELOPMENT
              </span>

            </div>

            <span class="column-count">
              {{ getDevelopmentIssues().length }}
            </span>

          </div>


          <div class="column-content">

            <div
              class="issue-card"
              *ngFor="let issue of getDevelopmentIssues()"
              (click)="openIssue(issue)">


              <div class="issue-card-top">

                <span class="issue-id">
                  Id: {{ issue.id }}
                </span>

                <span class="issue-date">
                  {{ issue.createdDate || '' }}
                </span>

              </div>


              <h3 class="issue-title">

                {{ issue.summary }}

              </h3>


              <p class="issue-description">

                {{ issue.description }}

              </p>


              <div class="issue-footer">

                <div class="issue-assignee">

                  <div class="mini-avatar">

                    <img
                      *ngIf="profileImage"
                      [src]="profileImage"
                      [alt]="assigneeName"
                    />

                    <span *ngIf="!profileImage">
                      {{ assigneeName.charAt(0).toUpperCase() }}
                    </span>

                  </div>

                  <span>
                    {{ assigneeName }}
                  </span>

                </div>


                <span
                  class="priority-badge"
                  [ngClass]="getPriorityClass(issue.priority)">

                  {{ issue.priority }}

                </span>

              </div>


              <div class="issue-bottom-info">

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


            <div
              class="column-empty"
              *ngIf="getDevelopmentIssues().length === 0">

              No issues

            </div>

          </div>

        </div>


        <!-- ===================================================
             TESTING
             =================================================== -->

        <div class="kanban-column">

          <div class="column-header testing-header">

            <div class="column-title">

              <span class="column-icon">
                ✓
              </span>

              <span>
                TESTING
              </span>

            </div>

            <span class="column-count">
              {{ getTestingIssues().length }}
            </span>

          </div>


          <div class="column-content">

            <div
              class="issue-card"
              *ngFor="let issue of getTestingIssues()"
              (click)="openIssue(issue)">


              <div class="issue-card-top">

                <span class="issue-id">
                  Id: {{ issue.id }}
                </span>

                <span class="issue-date">
                  {{ issue.createdDate || '' }}
                </span>

              </div>


              <h3 class="issue-title">

                {{ issue.summary }}

              </h3>


              <p class="issue-description">

                {{ issue.description }}

              </p>


              <div class="issue-footer">

                <div class="issue-assignee">

                  <div class="mini-avatar">

                    <img
                      *ngIf="profileImage"
                      [src]="profileImage"
                      [alt]="assigneeName"
                    />

                    <span *ngIf="!profileImage">
                      {{ assigneeName.charAt(0).toUpperCase() }}
                    </span>

                  </div>

                  <span>
                    {{ assigneeName }}
                  </span>

                </div>


                <span
                  class="priority-badge"
                  [ngClass]="getPriorityClass(issue.priority)">

                  {{ issue.priority }}

                </span>

              </div>


              <div class="issue-bottom-info">

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


            <div
              class="column-empty"
              *ngIf="getTestingIssues().length === 0">

              No issues

            </div>

          </div>

        </div>


        <!-- ===================================================
             COMPLETED
             =================================================== -->

        <div class="kanban-column">

          <div class="column-header completed-header">

            <div class="column-title">

              <span class="column-icon">
                ✓
              </span>

              <span>
                COMPLETED
              </span>

            </div>

            <span class="column-count">
              {{ getCompletedIssues().length }}
            </span>

          </div>


          <div class="column-content">

            <div
              class="issue-card"
              *ngFor="let issue of getCompletedIssues()"
              (click)="openIssue(issue)">


              <div class="issue-card-top">

                <span class="issue-id">
                  Id: {{ issue.id }}
                </span>

                <span class="issue-date">
                  {{ issue.createdDate || '' }}
                </span>

              </div>


              <h3 class="issue-title">

                {{ issue.summary }}

              </h3>


              <p class="issue-description">

                {{ issue.description }}

              </p>


              <div class="issue-footer">

                <div class="issue-assignee">

                  <div class="mini-avatar">

                    <img
                      *ngIf="profileImage"
                      [src]="profileImage"
                      [alt]="assigneeName"
                    />

                    <span *ngIf="!profileImage">
                      {{ assigneeName.charAt(0).toUpperCase() }}
                    </span>

                  </div>

                  <span>
                    {{ assigneeName }}
                  </span>

                </div>


                <span
                  class="priority-badge"
                  [ngClass]="getPriorityClass(issue.priority)">

                  {{ issue.priority }}

                </span>

              </div>


              <div class="issue-bottom-info">

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


            <div
              class="column-empty"
              *ngIf="getCompletedIssues().length === 0">

              No issues

            </div>

          </div>

        </div>

      </div>

    </section>

  </main>

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

  assigneeId: number = 0;

  assigneeName: string = 'Assignee';

  assigneeEmail: string = '';

  profileImage: string = '';


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

    const storedUser =
      localStorage.getItem('user');


    // -------------------------------------------------------
    // Check logged-in user
    // -------------------------------------------------------

    if (!storedUser) {

      this.router.navigate([
        '/login'
      ]);

      return;
    }


    // -------------------------------------------------------
    // Read user from localStorage
    // -------------------------------------------------------

    try {

      const user =
        JSON.parse(storedUser);


      this.assigneeId =
        Number(user.id);


      this.assigneeName =
        user.name || 'Assignee';


      this.assigneeEmail =
        user.email || '';


      /*
       * Your User model uses:
       *
       * profileImage
       *
       * So we read that value here.
       */

      this.profileImage =
        user.profileImage || '';


    } catch (error) {

      console.error(
        'Failed to read logged-in user:',
        error
      );

      this.errorMessage =
        'Invalid logged-in user information.';

      return;
    }


    // -------------------------------------------------------
    // Validate ID
    // -------------------------------------------------------

    if (!this.assigneeId) {

      this.errorMessage =
        'Logged-in user information is missing.';

      return;
    }


    // -------------------------------------------------------
    // Load assigned issues
    // -------------------------------------------------------

    this.loadIssues();
  }


  // =========================================================
  // LOAD ASSIGNED ISSUES
  // =========================================================

  loadIssues(): void {

    this.loading = true;

    this.errorMessage = '';

    this.successMessage = '';


    this.issueService
      .getIssuesByAssignee(this.assigneeId)
      .subscribe({

        next: (data: Issue[]) => {

          this.issues =
            data || [];

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
  // GO TO DASHBOARD
  // =========================================================

  goToDashboard(): void {

    /*
     * We are already on the dashboard.
     *
     * Scroll back to the top instead of
     * unnecessarily reloading the route.
     */

    window.scrollTo({
      top: 0,
      behavior: 'smooth'
    });
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


    return this.issues.filter(
      issue =>

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
  // TO DO ISSUES
  // =========================================================

  getTodoIssues(): Issue[] {

    return this.filteredIssues.filter(
      issue =>
        issue.status === 'OPEN'
    );
  }


  // =========================================================
  // DEVELOPMENT ISSUES
  // =========================================================

  getDevelopmentIssues(): Issue[] {

    return this.filteredIssues.filter(
      issue =>
        issue.status === 'IN_PROGRESS'
    );
  }


  // =========================================================
  // TESTING ISSUES
  // =========================================================

  getTestingIssues(): Issue[] {

    return this.filteredIssues.filter(
      issue =>
        issue.status === 'TESTING'
    );
  }


  // =========================================================
  // COMPLETED ISSUES
  // =========================================================

  getCompletedIssues(): Issue[] {

    return this.filteredIssues.filter(
      issue =>
        issue.status === 'CLOSED'
    );
  }


  // =========================================================
  // TOTAL COUNT
  // =========================================================

  getTotalIssues(): number {

    return this.issues.length;
  }


  // =========================================================
  // TO DO COUNT
  // =========================================================

  getTodoCount(): number {

    return this.issues.filter(
      issue =>
        issue.status === 'OPEN'
    ).length;
  }


  // =========================================================
  // DEVELOPMENT COUNT
  // =========================================================

  getDevelopmentCount(): number {

    return this.issues.filter(
      issue =>
        issue.status === 'IN_PROGRESS'
    ).length;
  }


  // =========================================================
  // TESTING COUNT
  // =========================================================

  getTestingCount(): number {

    return this.issues.filter(
      issue =>
        issue.status === 'TESTING'
    ).length;
  }


  // =========================================================
  // COMPLETED COUNT
  // =========================================================

  getCompletedCount(): number {

    return this.issues.filter(
      issue =>
        issue.status === 'CLOSED'
    ).length;
  }


  // =========================================================
  // OPEN ISSUE DETAILS
  // =========================================================

  openIssue(issue: Issue): void {

    if (
      issue.id === undefined ||
      issue.id === null
    ) {

      alert(
        'Issue ID is missing.'
      );

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

  getStatusClass(
    status: string
  ): string {

    if (
      status === 'OPEN'
    ) {

      return 'todo';
    }


    if (
      status === 'IN_PROGRESS'
    ) {

      return 'development';
    }


    if (
      status === 'TESTING'
    ) {

      return 'testing';
    }


    if (
      status === 'CLOSED'
    ) {

      return 'completed';
    }


    return '';
  }


  // =========================================================
  // PRIORITY CLASS
  // =========================================================

  getPriorityClass(
    priority: string
  ): string {

    if (
      priority === 'HIGH'
    ) {

      return 'high';
    }


    if (
      priority === 'MEDIUM'
    ) {

      return 'medium';
    }


    if (
      priority === 'LOW'
    ) {

      return 'low';
    }


    return '';
  }


  // =========================================================
  // LOGOUT
  // =========================================================

  logout(): void {

    localStorage.removeItem(
      'user'
    );


    this.router.navigate([
      '/login'
    ]);
  }

}
/* =========================================================
   GLOBAL LAYOUT
   ========================================================= */

* {
  box-sizing: border-box;
}

.app-layout {
  display: flex;
  min-height: 100vh;
  width: 100%;
  background: #f4f5f7;
  font-family: Arial, Helvetica, sans-serif;
  color: #333;
}


/* =========================================================
   SIDEBAR
   ========================================================= */

.sidebar {
  width: 210px;
  min-height: 100vh;

  background: #20252b;
  color: white;

  display: flex;
  flex-direction: column;

  position: sticky;
  top: 0;

  flex-shrink: 0;
}


/* =========================================================
   SIDEBAR PROFILE
   ========================================================= */

.sidebar-profile {
  padding: 25px 15px 20px;
  text-align: center;

  border-bottom: 1px solid #343a40;
}


.profile-image-wrapper {
  width: 72px;
  height: 72px;

  margin: 0 auto 12px;

  border-radius: 50%;

  overflow: hidden;

  border: 3px solid #ffffff;

  background: #ffc107;

  display: flex;
  align-items: center;
  justify-content: center;
}


.profile-image {
  width: 100%;
  height: 100%;

  object-fit: cover;
}


.profile-fallback {
  width: 100%;
  height: 100%;

  display: flex;
  align-items: center;
  justify-content: center;

  background: #ffc107;
  color: #20252b;

  font-size: 28px;
  font-weight: bold;
}


.sidebar-profile h3 {
  margin: 5px 0;

  font-size: 15px;
  color: #ffffff;
}


.profile-email {
  margin: 0;

  font-size: 11px;

  color: #ffc107;

  word-break: break-word;
}


/* =========================================================
   SIDEBAR STATISTICS
   ========================================================= */

.sidebar-stats {
  text-align: center;

  padding: 18px 10px;

  border-bottom: 1px solid #343a40;
}


.sidebar-stat-number {
  font-size: 24px;

  font-weight: bold;

  color: white;
}


.sidebar-stat-label {
  margin-top: 5px;

  font-size: 11px;

  color: #bbbbbb;
}


/* =========================================================
   SIDEBAR NAVIGATION
   ========================================================= */

.sidebar-navigation {
  padding: 15px 10px;
}


.nav-item {
  width: 100%;

  display: flex;
  align-items: center;

  gap: 9px;

  padding: 11px 8px;

  border: none;

  background: transparent;

  color: #ffc107;

  text-align: left;

  font-size: 12px;

  cursor: pointer;

  border-radius: 4px;
}


.nav-item:hover,
.nav-item.active {
  background: #2b3036;
}


.nav-icon {
  width: 18px;

  display: inline-flex;

  justify-content: center;

  font-size: 14px;
}


/* =========================================================
   SIDEBAR BOTTOM
   ========================================================= */

.sidebar-bottom {
  margin-top: auto;

  padding: 15px 12px 25px;
}


.logout-button {
  width: 100%;

  padding: 8px 10px;

  border: 1px solid #ffc107;

  border-radius: 4px;

  background: transparent;

  color: #ffc107;

  cursor: pointer;

  font-size: 11px;
}


.logout-button:hover {
  background: #ffc107;
  color: #20252b;
}


/* =========================================================
   MAIN CONTENT
   ========================================================= */

.main-content {
  flex: 1;

  min-width: 0;

  background: #f4f5f7;
}


/* =========================================================
   TOP HEADER
   ========================================================= */

.top-header {
  height: 55px;

  display: flex;
  align-items: center;
  justify-content: space-between;

  gap: 20px;

  padding: 0 20px;

  background: #ffc107;

  border-bottom: 1px solid #e0a800;
}


/* =========================================================
   SEARCH
   ========================================================= */

.search-container {
  display: flex;

  width: 55%;
  max-width: 600px;
}


.search-input {
  width: 100%;

  height: 32px;

  padding: 7px 10px;

  border: 1px solid #dddddd;

  border-right: none;

  border-radius: 4px 0 0 4px;

  outline: none;

  font-size: 12px;

  background: white;
}


.search-input:focus {
  border-color: #999;
}


.search-button {
  width: 38px;

  height: 32px;

  border: 1px solid #dddddd;

  border-radius: 0 4px 4px 0;

  background: #eeeeee;

  cursor: pointer;
}


.search-button:hover {
  background: #e0e0e0;
}


/* =========================================================
   APPLICATION NAME
   ========================================================= */

.application-name {
  font-size: 13px;

  font-weight: bold;

  color: #333;

  white-space: nowrap;
}


/* =========================================================
   CONTENT AREA
   ========================================================= */

.content-area {
  padding: 25px 30px 40px;
}


/* =========================================================
   PAGE HEADER
   ========================================================= */

.page-header {
  display: flex;

  justify-content: space-between;
  align-items: center;

  gap: 20px;

  margin-bottom: 25px;
}


.page-header h1 {
  margin: 0;

  font-size: 22px;

  color: #222;
}


.page-header p {
  margin: 6px 0 0;

  font-size: 13px;

  color: #777;
}


/* =========================================================
   REFRESH
   ========================================================= */

.refresh-button {
  padding: 9px 15px;

  border: none;

  border-radius: 4px;

  background: #1976d2;

  color: white;

  font-size: 12px;

  cursor: pointer;
}


.refresh-button:hover {
  background: #125ea8;
}


/* =========================================================
   MESSAGES
   ========================================================= */

.error-message {
  padding: 12px 15px;

  margin-bottom: 20px;

  background: #ffebee;

  color: #c62828;

  border: 1px solid #ffcdd2;

  border-radius: 5px;

  font-size: 13px;
}


.success-message {
  padding: 12px 15px;

  margin-bottom: 20px;

  background: #e8f5e9;

  color: #2e7d32;

  border: 1px solid #c8e6c9;

  border-radius: 5px;

  font-size: 13px;
}


/* =========================================================
   SUMMARY CARDS
   ========================================================= */

.summary-cards {
  display: grid;

  grid-template-columns: repeat(5, 1fr);

  gap: 12px;

  margin-bottom: 25px;
}


.summary-card {
  background: white;

  border: 1px solid #e2e2e2;

  border-radius: 5px;

  padding: 15px;

  text-align: center;
}


.summary-number {
  font-size: 23px;

  font-weight: bold;

  color: #333;
}


.summary-label {
  margin-top: 5px;

  font-size: 11px;

  color: #777;
}


/* Summary accents */

.todo-summary {
  border-top: 3px solid #d32f2f;
}


.development-summary {
  border-top: 3px solid #ef6c00;
}


.testing-summary {
  border-top: 3px solid #1976d2;
}


.completed-summary {
  border-top: 3px solid #2e7d32;
}


/* =========================================================
   LOADING / EMPTY
   ========================================================= */

.loading,
.empty-message {
  padding: 45px 20px;

  text-align: center;

  background: white;

  border: 1px solid #e0e0e0;

  border-radius: 6px;

  color: #777;
}


.empty-title {
  font-size: 18px;

  font-weight: bold;

  color: #555;

  margin-bottom: 7px;
}


.empty-message p {
  margin: 0;

  font-size: 13px;
}


/* =========================================================
   KANBAN BOARD
   ========================================================= */

.kanban {
  display: grid;

  grid-template-columns: repeat(4, minmax(0, 1fr));

  gap: 12px;

  align-items: start;
}


.kanban-column {
  min-width: 0;

  background: #eeeeee;

  border-radius: 3px;

  padding-bottom: 10px;
}


/* =========================================================
   COLUMN HEADER
   ========================================================= */

.column-header {
  min-height: 42px;

  display: flex;

  align-items: center;

  justify-content: space-between;

  padding: 8px 10px;

  background: #f7f7f7;

  border-top: 3px solid;

  border-bottom: 1px solid #dddddd;
}


.column-title {
  display: flex;

  align-items: center;

  gap: 6px;

  font-size: 11px;

  font-weight: bold;

  color: #555;
}


.column-icon {
  font-size: 11px;
}


.column-count {
  min-width: 20px;

  height: 20px;

  display: flex;

  align-items: center;
  justify-content: center;

  padding: 0 6px;

  border-radius: 50%;

  background: #dddddd;

  font-size: 10px;

  font-weight: bold;
}


/* Column colors */

.todo-header {
  border-top-color: #d32f2f;
}


.development-header {
  border-top-color: #ef6c00;
}


.testing-header {
  border-top-color: #1976d2;
}


.completed-header {
  border-top-color: #2e7d32;
}


/* =========================================================
   COLUMN CONTENT
   ========================================================= */

.column-content {
  padding: 8px;
}


/* =========================================================
   ISSUE CARD
   ========================================================= */

.issue-card {
  padding: 12px;

  margin-bottom: 9px;

  background: white;

  border: 1px solid #dddddd;

  border-radius: 4px;

  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08);

  cursor: pointer;

  transition:
    box-shadow 0.15s ease,
    transform 0.15s ease;
}


.issue-card:hover {
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.13);

  transform: translateY(-1px);
}


/* =========================================================
   ISSUE TOP
   ========================================================= */

.issue-card-top {
  display: flex;

  justify-content: space-between;

  gap: 5px;

  margin-bottom: 8px;
}


.issue-id {
  font-size: 10px;

  color: #666;

  font-weight: bold;
}


.issue-date {
  font-size: 9px;

  color: #999;
}


/* =========================================================
   ISSUE TITLE
   ========================================================= */

.issue-title {
  margin: 0 0 7px;

  font-size: 13px;

  line-height: 1.35;

  color: #333;
}


.issue-card:hover .issue-title {
  color: #1976d2;
}


/* =========================================================
   ISSUE DESCRIPTION
   ========================================================= */

.issue-description {
  margin: 0;

  color: #777;

  font-size: 10px;

  line-height: 1.45;

  display: -webkit-box;

  -webkit-line-clamp: 3;

  -webkit-box-orient: vertical;

  overflow: hidden;
}


/* =========================================================
   ISSUE FOOTER
   ========================================================= */

.issue-footer {
  display: flex;

  justify-content: space-between;

  align-items: center;

  gap: 8px;

  margin-top: 12px;

  padding-top: 9px;

  border-top: 1px solid #eeeeee;
}


.issue-assignee {
  display: flex;

  align-items: center;

  gap: 5px;

  min-width: 0;

  font-size: 9px;

  color: #666;
}


.issue-assignee > span {
  overflow: hidden;

  text-overflow: ellipsis;

  white-space: nowrap;
}


/* =========================================================
   MINI AVATAR
   ========================================================= */

.mini-avatar {
  width: 23px;
  height: 23px;

  flex-shrink: 0;

  overflow: hidden;

  border-radius: 50%;

  background: #ffc107;

  display: flex;

  align-items: center;
  justify-content: center;

  font-size: 10px;

  font-weight: bold;

  color: #333;
}


.mini-avatar img {
  width: 100%;
  height: 100%;

  object-fit: cover;
}


/* =========================================================
   PRIORITY
   ========================================================= */

.priority-badge {
  padding: 3px 6px;

  border-radius: 3px;

  font-size: 8px;

  font-weight: bold;

  white-space: nowrap;
}


.priority-badge.high {
  background: #ffebee;

  color: #d32f2f;
}


.priority-badge.medium {
  background: #fff3e0;

  color: #ef6c00;
}


.priority-badge.low {
  background: #e8f5e9;

  color: #2e7d32;
}


/* =========================================================
   ISSUE BOTTOM
   ========================================================= */

.issue-bottom-info {
  display: flex;

  justify-content: space-between;

  gap: 8px;

  margin-top: 8px;

  font-size: 9px;

  color: #777;
}


.issue-bottom-info strong {
  color: #555;
}


/* =========================================================
   EMPTY COLUMN
   ========================================================= */

.column-empty {
  padding: 20px 10px;

  text-align: center;

  font-size: 10px;

  color: #999;
}


/* =========================================================
   RESPONSIVE
   ========================================================= */

@media (max-width: 1100px) {

  .sidebar {
    width: 180px;
  }

  .content-area {
    padding: 20px;
  }

  .summary-cards {
    grid-template-columns: repeat(3, 1fr);
  }

  .kanban {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }

}


@media (max-width: 750px) {

  .app-layout {
    flex-direction: column;
  }


  .sidebar {
    width: 100%;

    min-height: auto;

    position: relative;
  }


  .sidebar-profile {
    display: flex;

    align-items: center;

    gap: 12px;

    text-align: left;

    padding: 12px 15px;
  }


  .profile-image-wrapper {
    width: 50px;
    height: 50px;

    margin: 0;
  }


  .profile-email {
    font-size: 10px;
  }


  .sidebar-stats {
    display: none;
  }


  .sidebar-navigation {
    display: none;
  }


  .sidebar-bottom {
    position: absolute;

    right: 10px;
    top: 12px;

    margin: 0;

    padding: 0;
  }


  .logout-button {
    width: auto;

    padding: 7px 12px;
  }


  .top-header {
    height: auto;

    padding: 10px 15px;

    flex-direction: column;

    align-items: stretch;

    gap: 8px;
  }


  .search-container {
    width: 100%;

    max-width: none;
  }


  .application-name {
    text-align: right;

    font-size: 11px;
  }


  .content-area {
    padding: 18px 15px;
  }


  .page-header {
    flex-direction: column;

    align-items: flex-start;
  }


  .summary-cards {
    grid-template-columns: repeat(2, 1fr);
  }


  .kanban {
    grid-template-columns: 1fr;
  }

}


@media (max-width: 450px) {

  .summary-cards {
    grid-template-columns: 1fr;
  }

  .page-header h1 {
    font-size: 20px;
  }

}
