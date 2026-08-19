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

  font-family:
    Arial,
    Helvetica,
    sans-serif;

  color: #333;
}



/* =========================================================
   SIDEBAR
   ========================================================= */

.sidebar {
  width: 220px;

  min-height: 100vh;

  position: fixed;

  left: 0;
  top: 0;
  bottom: 0;

  background: #20252b;

  color: white;

  padding: 20px 14px;

  z-index: 10;

  display: flex;

  flex-direction: column;
}



/* =========================================================
   PROFILE
   ========================================================= */

.profile-section {
  text-align: center;

  padding-bottom: 22px;

  border-bottom: 1px solid #343a40;
}


.profile-image {
  width: 78px;

  height: 78px;

  object-fit: cover;

  border-radius: 50%;

  border: 3px solid #ffffff;

  display: block;

  margin: 0 auto 10px auto;

  background: #eeeeee;
}


.profile-section h3 {
  margin: 5px 0;

  font-size: 16px;

  color: #ffffff;

  font-weight: 600;
}


.profile-email {
  margin: 0;

  color: #bfc3c7;

  font-size: 11px;

  word-break: break-word;
}



/* =========================================================
   PROFILE STATS
   ========================================================= */

.profile-stats {
  display: flex;

  justify-content: center;

  gap: 35px;

  margin-top: 18px;
}


.profile-stat {
  display: flex;

  flex-direction: column;

  align-items: center;
}


.profile-stat strong {
  font-size: 18px;

  color: #ffffff;
}


.profile-stat span {
  font-size: 10px;

  color: #bfc3c7;

  margin-top: 3px;
}



/* =========================================================
   SIDEBAR MENU
   ========================================================= */

.sidebar-menu {
  margin-top: 20px;
}


.sidebar-link {
  width: 100%;

  display: flex;

  align-items: center;

  gap: 8px;

  padding: 11px 8px;

  margin-bottom: 4px;

  border: none;

  border-radius: 4px;

  background: transparent;

  color: #f5c400;

  text-align: left;

  cursor: pointer;

  font-size: 12px;

  transition: background 0.2s ease;
}


.sidebar-link:hover {
  background: #30363d;
}


.sidebar-link.active {
  background: #2c3239;
}


.menu-icon {
  width: 18px;

  text-align: center;
}



/* =========================================================
   LOGOUT
   ========================================================= */

.sidebar-logout {
  margin-top: auto;

  width: 100%;

  padding: 8px;

  border: 1px solid #f5c400;

  border-radius: 5px;

  background: transparent;

  color: #f5c400;

  cursor: pointer;

  font-size: 11px;
}


.sidebar-logout:hover {
  background: #f5c400;

  color: #20252b;
}



/* =========================================================
   MAIN CONTENT
   ========================================================= */

.main-content {
  margin-left: 220px;

  width: calc(100% - 220px);

  min-height: 100vh;

  background: #f4f5f7;
}



/* =========================================================
   APPLICATION HEADER
   ========================================================= */

.app-header {
  height: 48px;

  width: 100%;

  background: #ffc400;

  display: flex;

  align-items: center;

  justify-content: space-between;

  padding: 0 20px;
}


.search-container {
  display: flex;

  width: 380px;

  height: 30px;
}


.search-container input {
  flex: 1;

  border: none;

  outline: none;

  padding: 0 10px;

  font-size: 11px;

  background: #ffffff;
}


.search-button {
  width: 36px;

  border: none;

  background: #e5aa00;

  cursor: pointer;

  font-size: 12px;
}


.search-button:hover {
  background: #d99f00;
}


.application-name {
  font-size: 13px;

  font-weight: bold;

  color: #333;
}



/* =========================================================
   DASHBOARD
   ========================================================= */

.dashboard {
  width: 100%;

  max-width: 1400px;

  margin: 0 auto;

  padding: 25px;
}



/* =========================================================
   DASHBOARD HEADING
   ========================================================= */

.dashboard-heading {
  display: flex;

  justify-content: space-between;

  align-items: center;

  margin-bottom: 20px;
}


.dashboard h1 {
  margin: 0;

  font-size: 27px;

  color: #333;
}


.subtitle {
  margin: 5px 0 0 0;

  color: #777;

  font-size: 13px;
}


.refresh-main-btn {
  padding: 9px 16px;

  border: none;

  border-radius: 5px;

  background: #1976d2;

  color: white;

  cursor: pointer;

  font-size: 12px;
}


.refresh-main-btn:hover {
  background: #125ea8;
}



/* =========================================================
   ERROR
   ========================================================= */

.error-message {
  padding: 12px 16px;

  margin-bottom: 18px;

  border-radius: 5px;

  background: #ffebee;

  color: #c62828;

  border: 1px solid #ffcdd2;

  font-size: 13px;
}



/* =========================================================
   SUCCESS
   ========================================================= */

.success-message {
  padding: 12px 16px;

  margin-bottom: 18px;

  border-radius: 5px;

  background: #e8f5e9;

  color: #2e7d32;

  border: 1px solid #c8e6c9;

  font-size: 13px;
}



/* =========================================================
   PROJECT SELECTOR
   ========================================================= */

.project-selector {
  display: flex;

  justify-content: space-between;

  align-items: flex-end;

  gap: 25px;

  padding: 15px;

  margin-bottom: 18px;

  background: white;

  border: 1px solid #ddd;

  border-radius: 5px;
}


.selector-left {
  display: flex;

  flex-direction: column;

  gap: 6px;

  width: 45%;
}


.selector-left label {
  font-size: 11px;

  font-weight: bold;

  color: #555;
}


.selector-left select {
  width: 100%;

  padding: 9px;

  border: 1px solid #ddd;

  border-radius: 4px;

  background: white;

  font-size: 12px;

  outline: none;
}


.owner-name-display {
  display: flex;

  flex-direction: column;

  gap: 5px;

  font-size: 11px;

  color: #777;
}


.owner-name-display strong {
  color: #333;

  font-size: 13px;
}



/* =========================================================
   PROJECT CARD
   ========================================================= */

.project-card {
  background: white;

  border: 1px solid #ddd;

  border-radius: 5px;

  padding: 20px;

  margin-bottom: 30px;
}



/* =========================================================
   PROJECT INFORMATION
   ========================================================= */

.project-information {
  display: flex;

  justify-content: space-between;

  align-items: flex-start;

  padding-bottom: 18px;

  border-bottom: 1px solid #e5e5e5;
}


.project-info-left h2 {
  margin: 0 0 10px 0;

  font-size: 20px;

  color: #333;
}


.project-info-left p {
  margin: 5px 0;

  font-size: 12px;

  color: #666;
}


.project-dates {
  display: flex;

  gap: 35px;
}


.project-dates div {
  display: flex;

  flex-direction: column;

  gap: 5px;
}


.project-dates span {
  font-size: 10px;

  color: #888;
}


.project-dates strong {
  font-size: 12px;

  color: #333;
}



/* =========================================================
   PROJECT ACTIONS
   ========================================================= */

.project-actions {
  display: flex;

  gap: 8px;

  margin: 15px 0;
}


.edit-button,
.delete-button {
  padding: 8px 14px;

  border: none;

  border-radius: 4px;

  cursor: pointer;

  font-size: 11px;

  font-weight: 600;
}


.edit-button {
  background: #1976d2;

  color: white;
}


.edit-button:hover {
  background: #125ea8;
}


.delete-button {
  background: #d32f2f;

  color: white;
}


.delete-button:hover {
  background: #b71c1c;
}



/* =========================================================
   STATISTICS
   ========================================================= */

.stats {
  display: grid;

  grid-template-columns:
    repeat(5, 1fr);

  gap: 10px;

  margin: 18px 0;
}


.stat-card {
  padding: 14px;

  text-align: center;

  border: 1px solid #e2e2e2;

  background: #fafafa;

  border-radius: 5px;
}


.stat-number {
  font-size: 23px;

  font-weight: bold;

  color: #1976d2;

  margin-bottom: 3px;
}


.stat-label {
  font-size: 10px;

  color: #777;
}



/* =========================================================
   FILTERS
   ========================================================= */

.filters {
  display: flex;

  gap: 7px;

  flex-wrap: wrap;

  padding: 12px 0;

  margin-bottom: 12px;
}


.filters button {
  padding: 7px 13px;

  border: 1px solid #ccc;

  border-radius: 4px;

  background: white;

  color: #555;

  cursor: pointer;

  font-size: 11px;
}


.filters button:hover {
  background: #f1f1f1;
}


.filters button.active {
  background: #ffc400;

  border-color: #e0a900;

  color: #333;

  font-weight: bold;
}



/* =========================================================
   ISSUES SECTION
   ========================================================= */

.issues-section {
  margin-top: 15px;
}


.issues-heading {
  display: flex;

  justify-content: space-between;

  align-items: center;

  margin-bottom: 12px;
}


.issues-heading h2 {
  margin: 0;

  font-size: 18px;

  color: #333;
}


.issues-heading span {
  font-size: 11px;

  color: #888;
}



/* =========================================================
   KANBAN BOARD
   ========================================================= */

.kanban-board {
  display: grid;

  grid-template-columns:
    repeat(3, minmax(0, 1fr));

  gap: 12px;

  width: 100%;
}


.kanban-column {
  min-width: 0;

  background: #f5f6f7;

  border-radius: 4px;

  padding: 8px;

  min-height: 250px;
}



/* =========================================================
   KANBAN HEADERS
   ========================================================= */

.kanban-column-header {
  display: flex;

  justify-content: space-between;

  align-items: center;

  padding: 9px 10px;

  margin-bottom: 8px;

  background: white;

  border-top: 3px solid;

  border-radius: 3px;

  font-size: 11px;

  font-weight: bold;
}


.open-header {
  border-color: #e53935;
}


.progress-header {
  border-color: #1976d2;
}


.closed-header {
  border-color: #43a047;
}


.column-count {
  display: inline-flex;

  align-items: center;

  justify-content: center;

  min-width: 19px;

  height: 19px;

  padding: 0 5px;

  border-radius: 50%;

  background: #eee;

  font-size: 10px;
}



/* =========================================================
   KANBAN CARDS
   ========================================================= */

.kanban-cards {
  display: flex;

  flex-direction: column;

  gap: 8px;
}


.kanban-issue {
  background: white;

  border: 1px solid #ddd;

  border-radius: 4px;

  padding: 11px;

  box-shadow:
    0 1px 3px rgba(0, 0, 0, 0.06);

  transition:
    transform 0.15s ease,
    box-shadow 0.15s ease;
}


.kanban-issue:hover {
  transform: translateY(-2px);

  box-shadow:
    0 3px 8px rgba(0, 0, 0, 0.10);
}



/* =========================================================
   ISSUE CARD HEADER
   ========================================================= */

.issue-card-header {
  display: flex;

  justify-content: space-between;

  align-items: center;

  gap: 5px;

  margin-bottom: 8px;
}


.issue-number {
  font-size: 9px;

  color: #888;
}


.priority-badge {
  padding: 3px 6px;

  border-radius: 3px;

  font-size: 8px;

  font-weight: bold;
}


.priority-badge.high {
  background: #ffebee;

  color: #c62828;
}


.priority-badge.medium {
  background: #fff8e1;

  color: #f57f17;
}


.priority-badge.low {
  background: #e8f5e9;

  color: #2e7d32;
}



/* =========================================================
   ISSUE TITLE
   ========================================================= */

.kanban-issue h3 {
  margin: 0 0 7px 0;

  font-size: 12px;

  line-height: 1.4;

  color: #333;

  cursor: pointer;
}


.kanban-issue h3:hover {
  color: #1976d2;

  text-decoration: underline;
}



/* =========================================================
   DESCRIPTION
   ========================================================= */

.issue-description {
  margin: 0 0 9px 0;

  font-size: 10px;

  line-height: 1.4;

  color: #777;

  display: -webkit-box;

  -webkit-line-clamp: 3;

  -webkit-box-orient: vertical;

  overflow: hidden;
}



/* =========================================================
   ISSUE META
   ========================================================= */

.issue-meta {
  display: flex;

  justify-content: space-between;

  gap: 5px;

  padding-top: 8px;

  border-top: 1px solid #eee;

  font-size: 9px;

  color: #777;
}


.issue-assignee {
  display: flex;

  justify-content: space-between;

  margin-top: 7px;

  font-size: 9px;

  color: #777;
}


.issue-assignee strong {
  color: #333;
}



/* =========================================================
   CARD CONTROLS
   ========================================================= */

.card-controls {
  display: flex;

  gap: 5px;

  margin-top: 9px;
}


.card-controls select {
  flex: 1;

  min-width: 0;

  padding: 5px;

  border: 1px solid #ddd;

  border-radius: 3px;

  background: white;

  font-size: 8px;

  cursor: pointer;

  outline: none;
}


.card-controls select:focus {
  border-color: #1976d2;
}



/* =========================================================
   NO ISSUES
   ========================================================= */

.no-issues {
  text-align: center;

  padding: 40px 20px;

  color: #777;

  background: #fafafa;

  border: 1px solid #e0e0e0;

  border-radius: 5px;

  font-size: 13px;
}



/* =========================================================
   NO PROJECTS
   ========================================================= */

.no-projects-screen {
  background: white;

  border: 1px solid #ddd;

  border-radius: 5px;

  min-height: 400px;

  display: flex;

  flex-direction: column;

  justify-content: center;

  align-items: center;

  text-align: center;
}


.no-projects-icon {
  font-size: 45px;

  margin-bottom: 15px;
}


.no-projects-screen h2 {
  margin: 0 0 7px 0;

  color: #e53935;

  font-size: 18px;
}


.no-projects-screen p {
  color: #777;

  font-size: 12px;
}


.create-project-empty-btn {
  margin-top: 10px;

  padding: 8px 14px;

  border: 1px solid #ffc400;

  background: #fff;

  color: #b88600;

  border-radius: 4px;

  cursor: pointer;

  font-size: 11px;
}


.create-project-empty-btn:hover {
  background: #ffc400;

  color: #333;
}



/* =========================================================
   EDIT PROJECT
   ========================================================= */

.edit-project-card {
  max-width: 700px;

  margin: 20px auto;

  padding: 25px;

  background: white;

  border-radius: 6px;

  border: 1px solid #ddd;

  box-shadow:
    0 2px 10px rgba(0, 0, 0, 0.08);
}


.edit-project-card h2 {
  margin-top: 0;

  margin-bottom: 25px;

  font-size: 20px;
}


.form-group {
  margin-bottom: 18px;
}


.form-group label {
  display: block;

  margin-bottom: 6px;

  font-size: 12px;

  font-weight: 600;
}


.form-group input {
  width: 100%;

  padding: 10px;

  border: 1px solid #ccc;

  border-radius: 4px;

  font-size: 13px;

  outline: none;
}


.form-group input:focus {
  border-color: #1976d2;
}


.edit-buttons {
  display: flex;

  gap: 10px;

  margin-top: 20px;
}


.save-button,
.cancel-button {
  padding: 9px 16px;

  border: none;

  border-radius: 4px;

  cursor: pointer;

  font-weight: 600;
}


.save-button {
  background: #1976d2;

  color: white;
}


.cancel-button {
  background: #ddd;

  color: #333;
}



/* =========================================================
   RESPONSIVE
   ========================================================= */

@media (max-width: 1100px) {

  .kanban-board {
    grid-template-columns:
      repeat(3, minmax(0, 1fr));
  }

}


@media (max-width: 900px) {

  .sidebar {
    width: 190px;
  }


  .main-content {
    margin-left: 190px;

    width: calc(100% - 190px);
  }


  .stats {
    grid-template-columns:
      repeat(3, 1fr);
  }


  .kanban-board {
    grid-template-columns: 1fr;
  }


  .project-information {
    flex-direction: column;

    gap: 20px;
  }


  .project-dates {
    text-align: left;
  }

}


@media (max-width: 768px) {

  .sidebar {
    position: relative;

    width: 100%;

    min-height: auto;

    padding: 15px;
  }


  .main-content {
    margin-left: 0;

    width: 100%;
  }


  .app-layout {
    flex-direction: column;
  }


  .sidebar-menu {
    display: flex;

    flex-wrap: wrap;

    gap: 5px;
  }


  .sidebar-link {
    width: auto;

    flex: 1;
  }


  .sidebar-logout {
    position: static;

    margin-top: 15px;
  }


  .app-header {
    flex-direction: column;

    height: auto;

    gap: 10px;

    padding: 10px;
  }


  .search-container {
    width: 100%;
  }


  .application-name {
    align-self: flex-end;
  }


  .dashboard {
    padding: 15px;
  }


  .dashboard-heading {
    flex-direction: column;

    align-items: stretch;

    gap: 10px;
  }


  .refresh-main-btn {
    align-self: flex-start;
  }


  .project-selector {
    flex-direction: column;

    align-items: stretch;
  }


  .selector-left {
    width: 100%;
  }


  .stats {
    grid-template-columns: 1fr 1fr;
  }

}


@media (max-width: 500px) {

  .stats {
    grid-template-columns: 1fr;
  }


  .project-actions {
    flex-direction: column;
  }


  .edit-button,
  .delete-button {
    width: 100%;
  }


  .project-dates {
    flex-direction: column;

    gap: 10px;
  }


  .filters {
    flex-direction: column;
  }


  .filters button {
    width: 100%;
  }


  .sidebar-menu {
    flex-direction: column;
  }


  .sidebar-link {
    width: 100%;
  }

}
