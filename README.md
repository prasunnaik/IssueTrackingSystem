.header-buttons {
  display: flex;
  gap: 10px;
  align-items: center;
}

.project-actions {
  display: flex;
  gap: 10px;
  margin: 20px 0;
}

.edit-button,
.delete-button,
.save-button,
.cancel-button {
  padding: 9px 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.edit-button,
.save-button {
  background: #1976d2;
  color: white;
}

.delete-button {
  background: #d32f2f;
  color: white;
}

.cancel-button {
  background: #ddd;
  color: #333;
}

.edit-project-card {
  max-width: 700px;
  margin: 20px auto;
  padding: 25px;
  background: white;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
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
}

.edit-buttons {
  display: flex;
  gap: 10px;
  margin-top: 20px;
}

.success-message {
  padding: 12px;
  margin: 15px 0;
  border-radius: 5px;
  background: #e8f5e9;
  color: #2e7d32;
}

.error-message {
  padding: 12px;
  margin: 15px 0;
  border-radius: 5px;
  background: #ffebee;
  color: #c62828;
}
