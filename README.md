.create-issue-container {
  min-height: 100vh;
  padding: 40px;
  background: #f5f6fa;
}

.issue-card {
  max-width: 700px;
  margin: 0 auto;
  padding: 30px;
  background: white;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.issue-card h2 {
  margin-bottom: 5px;
  color: #333;
}

.subtitle {
  margin-bottom: 25px;
  color: #777;
}

.form-group {
  margin-bottom: 18px;
}

.form-group label {
  display: block;
  margin-bottom: 7px;
  font-weight: 600;
  color: #333;
}

.form-group input,
.form-group textarea,
.form-group select {
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
.form-group textarea:focus,
.form-group select:focus {
  outline: none;
  border-color: #1976d2;
}

.button-container {
  display: flex;
  gap: 10px;
  margin-top: 25px;
}

.create-button,
.back-button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.create-button {
  background: #1976d2;
  color: white;
}

.create-button:hover {
  background: #125aa0;
}

.create-button:disabled {
  background: #aaa;
  cursor: not-allowed;
}

.back-button {
  background: #ddd;
  color: #333;
}

.back-button:hover {
  background: #ccc;
}

.error-message {
  padding: 10px;
  margin-bottom: 20px;
  background: #ffebee;
  color: #c62828;
  border-radius: 5px;
}

.success-message {
  padding: 10px;
  margin-bottom: 20px;
  background: #e8f5e9;
  color: #2e7d32;
  border-radius: 5px;
}
