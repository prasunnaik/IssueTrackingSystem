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
