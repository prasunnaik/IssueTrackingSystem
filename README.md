.page {
  min-height: 100vh;
  background: #f5f6f8;
  padding: 30px;
  box-sizing: border-box;
}


/* HEADER */

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 25px;
}

.header h1 {
  margin: 0;
  font-size: 28px;
  color: #222;
}

.header p {
  margin-top: 6px;
  color: #666;
}


/* BUTTONS */

.back-button,
.save-button,
.cancel-button {
  border: none;
  border-radius: 5px;
  padding: 10px 18px;
  cursor: pointer;
  font-size: 14px;
}

.back-button {
  background: #555;
  color: white;
}

.save-button {
  background: #f5b400;
  color: #222;
}

.save-button:disabled {
  background: #ccc;
  color: #777;
  cursor: not-allowed;
}

.cancel-button {
  background: #ddd;
  color: #333;
}


/* MESSAGES */

.loading {
  background: white;
  padding: 25px;
  text-align: center;
  border-radius: 8px;
}

.error-message {
  background: #ffebee;
  color: #c62828;
  padding: 12px 15px;
  border-radius: 5px;
  margin-bottom: 15px;
}

.success-message {
  background: #e8f5e9;
  color: #2e7d32;
  padding: 12px 15px;
  border-radius: 5px;
  margin-bottom: 15px;
}


/* ISSUE CONTAINER */

.issue-container {
  background: white;
  border-radius: 10px;
  padding: 30px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.08);
}


/* ISSUE HEADER */

.issue-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  border-bottom: 1px solid #ddd;
  padding-bottom: 20px;
}

.breadcrumb {
  font-size: 13px;
  color: #888;
  margin-bottom: 10px;
}

.issue-header h2 {
  margin: 0;
  color: #1976d2;
  font-size: 24px;
}


/* STATUS */

.status-badge {
  padding: 7px 14px;
  border-radius: 5px;
  font-size: 13px;
  font-weight: bold;
}

.status-badge.open {
  background: #e3f2fd;
  color: #1565c0;
}

.status-badge.progress {
  background: #fff3e0;
  color: #ef6c00;
}

.status-badge.closed {
  background: #e8f5e9;
  color: #2e7d32;
}


/* DESCRIPTION */

.description-section {
  padding: 20px 0;
  border-bottom: 1px solid #ddd;
}

.description-section h3 {
  margin-top: 0;
  font-size: 16px;
}

.description-section p {
  color: #555;
  line-height: 1.6;
}


/* DETAILS */

.details-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  padding: 25px 0;
}

.detail-item {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.label {
  font-size: 13px;
  color: #777;
  font-weight: 600;
}

.value {
  font-size: 15px;
  color: #333;
}


/* PRIORITY */

.priority {
  font-weight: bold;
}

.priority.high {
  color: #d32f2f;
}

.priority.medium {
  color: #ef6c00;
}

.priority.low {
  color: #2e7d32;
}


/* STATUS UPDATE */

.status-update {
  border-top: 1px solid #ddd;
  padding-top: 25px;
  display: flex;
  align-items: center;
  gap: 15px;
}

.status-update label {
  font-weight: 600;
}

.status-update select {
  padding: 9px 12px;
  border: 1px solid #bbb;
  border-radius: 5px;
  min-width: 180px;
}


/* ACTIONS */

.actions {
  display: flex;
  gap: 10px;
  margin-top: 25px;
}


/* MOBILE */

@media (max-width: 700px) {

  .page {
    padding: 15px;
  }

  .header {
    flex-direction: column;
    align-items: flex-start;
    gap: 15px;
  }

  .issue-header {
    flex-direction: column;
    gap: 15px;
  }

  .details-grid {
    grid-template-columns: 1fr;
  }

}
