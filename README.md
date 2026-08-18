.edit-page {
  max-width: 900px;
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
}

.page-header p {
  color: #666;
}

form {
  background: white;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  font-weight: 600;
  margin-bottom: 7px;
}

.form-group input,
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 10px;
  box-sizing: border-box;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 14px;
}

.form-group textarea {
  resize: vertical;
}

.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
  outline: none;
  border-color: #1976d2;
}

.form-group input[readonly] {
  background: #f3f3f3;
}

.form-group small {
  display: block;
  margin-top: 5px;
  color: #777;
}

.form-buttons {
  display: flex;
  gap: 10px;
  margin-top: 30px;
}

.save-button,
.reset-button,
.cancel-button {
  padding: 11px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-weight: 600;
}

.save-button {
  background: #1976d2;
  color: white;
}

.reset-button {
  background: #777;
  color: white;
}

.cancel-button {
  background: #ddd;
  color: #333;
}

.save-button:disabled,
.reset-button:disabled,
.cancel-button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.success-message {
  padding: 12px;
  margin-bottom: 20px;
  border-radius: 5px;
  background: #e8f5e9;
  color: #2e7d32;
}

.error-message {
  padding: 12px;
  margin-bottom: 20px;
  border-radius: 5px;
  background: #ffebee;
  color: #c62828;
}

.loading {
  padding: 20px;
  text-align: center;
}
