.issue-details-page {
  max-width: 1000px;
  margin: 30px auto;
  padding: 20px;
}

.page-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 25px;
}

.page-header h1 {
  margin: 0;
  color: #222;
}

.page-header p {
  color: #666;
}

.back-button {
  padding: 10px 18px;
  border: none;
  border-radius: 5px;
  background: #555;
  color: white;
  cursor: pointer;
}

.loading {
  padding: 20px;
  text-align: center;
}

.error-message {
  padding: 15px;
  margin-bottom: 20px;
  background: #ffebee;
  color: #c62828;
  border-radius: 5px;
}

.issue-card {
  background: white;
  border-radius: 10px;
  padding: 30px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.issue-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  border-bottom: 1px solid #ddd;
  padding-bottom: 20px;
}

.issue-header h2 {
  margin: 0 0 8px;
}

.issue-id {
  color: #777;
}

.status-badge {
  padding: 7px 14px;
  border-radius: 5px;
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

.section {
  margin-top: 25px;
}

.section h3 {
  margin-bottom: 8px;
}

.details-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  margin-top: 30px;
}

.detail {
  display: flex;
  flex-direction: column;
  gap: 7px;
  padding: 15px;
  border: 1px solid #eee;
  border-radius: 6px;
}

.detail strong {
  color: #555;
}

.priority.high {
  color: #d32f2f;
  font-weight: bold;
}

.priority.medium {
  color: #ef6c00;
  font-weight: bold;
}

.priority.low {
  color: #2e7d32;
  font-weight: bold;
}

.actions {
  display: flex;
  gap: 12px;
  margin-top: 30px;
  padding-top: 20px;
  border-top: 1px solid #ddd;
}

.edit-button,
.delete-button {
  padding: 11px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-weight: 600;
}

.edit-button {
  background: #1976d2;
  color: white;
}

.delete-button {
  background: #d32f2f;
  color: white;
}

.edit-button:hover {
  background: #125ca1;
}

.delete-button:hover {
  background: #b71c1c;
}

@media (max-width: 700px) {

  .page-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 15px;
  }

  .details-grid {
    grid-template-columns: 1fr;
  }
}
