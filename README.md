/* =========================================================
   MAIN DASHBOARD
   ========================================================= */

.dashboard {
  max-width: 1100px;
  margin: 40px auto;
  padding: 30px;

  font-family: Arial, Helvetica, sans-serif;

  color: #333;
  background-color: #f7f9fc;

  min-height: 100vh;
}


/* =========================================================
   HEADER
   ========================================================= */

.dashboard-header {
  display: flex;

  justify-content: space-between;
  align-items: center;

  gap: 20px;

  margin-bottom: 30px;
}


.header-title {
  flex: 1;
}


.dashboard h1 {
  margin: 0 0 8px 0;

  font-size: 32px;

  color: #222;
}


.subtitle {
  margin: 0;

  color: #777;

  font-size: 15px;
}


/* =========================================================
   HEADER BUTTONS
   ========================================================= */

.header-buttons {
  display: flex;

  align-items: center;

  gap: 10px;

  flex-wrap: wrap;
}


.create-project-btn,
.create-issue-btn,
.refresh-btn {
  padding: 10px 16px;

  border: none;

  border-radius: 6px;

  color: white;

  font-size: 14px;

  font-weight: 600;

  cursor: pointer;
}


/* Create Project */

.create-project-btn {
  background-color: #1976d2;
}


.create-project-btn:hover {
  background-color: #125ea8;
}


/* Create Issue */

.create-issue-btn {
  background-color: #2e7d32;
}


.create-issue-btn:hover {
  background-color: #1b5e20;
}


/* Refresh */

.refresh-btn {
  background-color: #616161;
}


.refresh-btn:hover {
  background-color: #424242;
}


/* =========================================================
   ERROR
   ========================================================= */

.error-message {
  padding: 12px 16px;

  margin-bottom: 20px;

  border-radius: 6px;

  background-color: #ffebee;

  color: #c62828;

  border: 1px solid #ffcdd2;
}


/* =========================================================
   SUCCESS
   ========================================================= */

.success-message {
  padding: 12px 16px;

  margin-bottom: 20px;

  border-radius: 6px;

  background-color: #e8f5e9;

  color: #2e7d32;

  border: 1px solid #c8e6c9;
}


/* =========================================================
   PROJECT SELECTOR
   ========================================================= */

.project-selector {
  display: flex;

  align-items: center;

  gap: 12px;

  margin-bottom: 20px;

  padding: 15px;

  background-color: white;

  border: 1px solid #e0e0e0;

  border-radius: 8px;
}


.project-selector label {
  font-weight: 600;
}


.project-selector select {
  min-width: 250px;

  padding: 9px 12px;

  border: 1px solid #ccc;

  border-radius: 6px;

  background-color: white;

  cursor: pointer;
}


/* =========================================================
   EMPTY MESSAGE
   ========================================================= */

.empty-message,
.no-projects,
.no-issues {
  text-align: center;

  padding: 30px;

  color: #777;

  background-color: white;

  border-radius: 10px;

  border: 1px solid #e0e0e0;
}


/* =========================================================
   EDIT PROJECT
   ========================================================= */

.edit-project-card {
  max-width: 700px;

  margin: 20px auto;

  padding: 25px;

  background-color: white;

  border-radius: 10px;

  border: 1px solid #e0e0e0;

  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.08);
}


.edit-project-card h2 {
  margin-top: 0;

  margin-bottom: 25px;
}


.form-group {
  margin-bottom: 18px;
}


.form-group label {
  display: block;

  margin-bottom: 6px;

  font-weight: 600;
}


.form-group input {
  width: 100%;

  padding: 10px;

  box-sizing: border-box;

  border: 1px solid #ccc;

  border-radius: 5px;

  font-size: 14px;
}


.form-group input:focus {
  outline: none;

  border-color: #1976d2;
}


/* Edit Buttons */

.edit-buttons {
  display: flex;

  gap: 10px;

  margin-top: 20px;
}


.save-button,
.cancel-button {
  padding: 9px 16px;

  border: none;

  border-radius: 5px;

  cursor: pointer;

  font-weight: 600;
}


.save-button {
  background-color: #1976d2;

  color: white;
}


.save-button:hover {
  background-color: #125ea8;
}


.cancel-button {
  background-color: #ddd;

  color: #333;
}


.cancel-button:hover {
  background-color: #ccc;
}


/* =========================================================
   PROJECT CARD
   ========================================================= */

.project-card {
  background-color: white;

  border: 1px solid #e0e0e0;

  border-radius: 12px;

  padding: 25px;

  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);

  margin-bottom: 30px;
}


/* =========================================================
   PROJECT HEADER
   ========================================================= */

.project-header {
  display: flex;

  justify-content: space-between;

  align-items: flex-start;

  padding-bottom: 20px;

  border-bottom: 1px solid #e5e5e5;
}


.project-header h2 {
  margin: 0 0 8px 0;

  font-size: 24px;

  color: #333;
}


.project-header p {
  margin: 5px 0;

  color: #666;
}


.project-dates {
  text-align: right;
}


.project-dates p {
  margin: 5px 0;
}


/* =========================================================
   PROJECT ACTIONS
   ========================================================= */

.project-actions {
  display: flex;

  gap: 10px;

  margin: 20px 0;
}


.edit-button,
.delete-button {
  padding: 9px 16px;

  border: none;

  border-radius: 5px;

  cursor: pointer;

  font-weight: 600;
}


.edit-button {
  background-color: #1976d2;

  color: white;
}


.edit-button:hover {
  background-color: #125ea8;
}


.delete-button {
  background-color: #d32f2f;

  color: white;
}


.delete-button:hover {
  background-color: #b71c1c;
}


/* =========================================================
   STATISTICS
   ========================================================= */

.stats {
  display: grid;

  grid-template-columns:
    repeat(5, 1fr);

  gap: 15px;

  margin: 25px 0;
}


.stat-card {
  border: 1px solid #e0e0e0;

  border-radius: 8px;

  padding: 18px;

  text-align: center;

  background-color: #fafafa;
}


.stat-number {
  font-size: 28px;

  font-weight: bold;

  color: #1976d2;

  margin-bottom: 5px;
}


.stat-label {
  color: #777;

  font-size: 13px;
}


/* =========================================================
   FILTERS
   ========================================================= */

.filters {
  display: flex;

  flex-wrap: wrap;

  gap: 8px;

  margin: 25px 0;
}


.filters button {
  padding: 8px 14px;

  border: 1px solid #ccc;

  border-radius: 6px;

  background-color: white;

  color: #555;

  cursor: pointer;

  font-size: 13px;
}


.filters button:hover {
  background-color: #f0f0f0;
}


.filters button.active {
  background-color: #1976d2;

  color: white;

  border-color: #1976d2;
}


/* =========================================================
   ISSUES SECTION
   ========================================================= */

.issues-section {
  margin-top: 25px;
}


.issues-section h2 {
  font-size: 20px;

  margin-bottom: 15px;

  color: #333;
}


/* =========================================================
   ISSUE CARD
   ========================================================= */

.issue-card {
  border: 1px solid #e1e1e1;

  border-radius: 8px;

  padding: 20px;

  margin-bottom: 15px;

  background-color: #fff;
}


.issue-card:hover {
  box-shadow:
    0 2px 8px rgba(0, 0, 0, 0.06);
}


/* =========================================================
   ISSUE TOP
   ========================================================= */

.issue-top {
  display: flex;

  justify-content: space-between;

  align-items: center;

  gap: 15px;

  margin-bottom: 15px;
}


.issue-title {
  margin: 0;

  font-size: 18px;

  color: #1976d2;

  cursor: pointer;
}


.issue-title:hover {
  text-decoration: underline;
}


.issue-id {
  margin: 5px 0 0 0;

  font-size: 12px;

  color: #888;
}


/* =========================================================
   STATUS BADGE
   ========================================================= */

.status-badge {
  padding: 6px 12px;

  border-radius: 20px;

  font-size: 12px;

  font-weight: bold;

  white-space: nowrap;
}


/* Open */

.status-badge.OPEN {
  background-color: #e3f2fd;

  color: #1565c0;
}


/* In Progress */

.status-badge.IN_PROGRESS {
  background-color: #fff3e0;

  color: #ef6c00;
}


/* Closed */

.status-badge.CLOSED {
  background-color: #e8f5e9;

  color: #2e7d32;
}


/* =========================================================
   DESCRIPTION
   ========================================================= */

.description {
  margin: 8px 0;

  font-size: 14px;

  color: #555;

  line-height: 1.5;
}


/* =========================================================
   ISSUE DETAILS
   ========================================================= */

.issue-details {
  display: flex;

  flex-wrap: wrap;

  gap: 20px;

  margin-top: 15px;

  padding-top: 15px;

  border-top: 1px solid #eeeeee;
}


.issue-details span {
  font-size: 13px;

  color: #555;
}


.issue-details strong {
  margin-right: 5px;
}


.issue-details select {
  padding: 5px 8px;

  border: 1px solid #ccc;

  border-radius: 5px;

  background-color: white;

  cursor: pointer;
}


/* =========================================================
   STATUS UPDATE
   ========================================================= */

.status-update {
  display: flex;

  align-items: center;

  gap: 12px;

  margin-top: 20px;

  padding-top: 15px;

  border-top: 1px solid #eeeeee;
}


.status-update label {
  font-weight: 600;

  font-size: 14px;
}


.status-update select {
  padding: 8px 12px;

  border: 1px solid #ccc;

  border-radius: 6px;

  background-color: white;

  font-size: 14px;

  cursor: pointer;
}


.status-update select:focus,
.issue-details select:focus {
  outline: none;

  border-color: #1976d2;
}


/* =========================================================
   RESPONSIVE
   ========================================================= */

@media (max-width: 900px) {

  .stats {
    grid-template-columns:
      repeat(3, 1fr);
  }

}


@media (max-width: 768px) {

  .dashboard {
    margin: 10px;

    padding: 15px;
  }


  .dashboard-header {
    flex-direction: column;

    align-items: stretch;
  }


  .dashboard h1 {
    text-align: center;

    font-size: 26px;
  }


  .subtitle {
    text-align: center;
  }


  .header-buttons {
    justify-content: center;
  }


  .project-header {
    flex-direction: column;
  }


  .project-dates {
    text-align: left;

    margin-top: 15px;
  }


  .stats {
    grid-template-columns: 1fr;
  }


  .project-selector {
    flex-direction: column;

    align-items: stretch;
  }


  .project-selector select {
    width: 100%;

    min-width: unset;
  }


  .issue-top {
    flex-direction: column;

    align-items: flex-start;
  }


  .issue-details {
    flex-direction: column;

    gap: 10px;
  }


  .status-update {
    flex-direction: column;

    align-items: flex-start;
  }


  .project-actions {
    flex-wrap: wrap;
  }

}


@media (max-width: 500px) {

  .header-buttons {
    flex-direction: column;

    width: 100%;
  }


  .create-project-btn,
  .create-issue-btn,
  .refresh-btn {
    width: 100%;
  }


  .project-actions button {
    width: 100%;
  }

}
