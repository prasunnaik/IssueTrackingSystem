import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { RouterLink } from '@angular/router';

@Component({
  selector: 'app-login',
  standalone: true,
  imports: [FormsModule, RouterLink],
  templateUrl: './login.component.html',
  styleUrl: './login.component.css'
})
export class LoginComponent {

  email = '';
  password = '';
  role = '';

  login() {
    console.log('Login clicked');
    console.log('Email:', this.email);
    console.log('Password:', this.password);
    console.log('Role:', this.role);
  }
}



<div class="login-page">

  <div class="login-box">

    <div class="title">
      Issue Tracking System
    </div>

    <h2>Login</h2>

    <form #loginForm="ngForm" (ngSubmit)="login()">

      <!-- Email -->
      <div class="form-group">

        <label>Email address</label>

        <input
          type="email"
          name="email"
          [(ngModel)]="email"
          required
          email
          #emailField="ngModel"
          placeholder="Enter email">

        <div
          class="error"
          *ngIf="emailField.invalid && emailField.touched">

          <span *ngIf="emailField.errors?.['required']">
            Email is required
          </span>

          <span *ngIf="emailField.errors?.['email']">
            Enter a valid email address
          </span>

        </div>

      </div>


      <!-- Password -->
      <div class="form-group">

        <label>Password</label>

        <input
          type="password"
          name="password"
          [(ngModel)]="password"
          required
          #passwordField="ngModel"
          placeholder="Password">

        <div
          class="error"
          *ngIf="passwordField.invalid && passwordField.touched">

          Password is required

        </div>

      </div>


      <!-- Role -->
      <div class="form-group">

        <label>Role</label>

        <select
          name="role"
          [(ngModel)]="role"
          required
          #roleField="ngModel">

          <option value="">Select your role</option>
          <option value="PROJECT_OWNER">Project Owner</option>
          <option value="ASSIGNEE">Assignee</option>

        </select>

        <div
          class="error"
          *ngIf="roleField.invalid && roleField.touched">

          Role is required

        </div>

      </div>


      <!-- Login Button -->
      <button
        type="submit"
        [disabled]="loginForm.invalid">

        Login

      </button>

    </form>


    <!-- Signup -->
    <div class="signup-link">

      Don't have an account?

      <a routerLink="/signup">
        Signup
      </a>

    </div>

  </div>

</div>

.login-page {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #f5f5f5;
}

.login-box {
  width: 350px;
  padding: 25px;
  background-color: white;
  border: 1px solid #ddd;
  border-radius: 5px;
  box-shadow: 0 2px 8px #ccc;
}

.title {
  text-align: center;
  font-size: 22px;
  font-weight: bold;
  margin-bottom: 20px;
}

h2 {
  text-align: center;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 15px;
}

label {
  display: block;
  margin-bottom: 5px;
  font-size: 14px;
}

input,
select {
  width: 100%;
  padding: 9px;
  box-sizing: border-box;
  border: 1px solid #ccc;
  border-radius: 3px;
}

button {
  width: 100%;
  padding: 10px;
  border: none;
  background-color: #ffc107;
  cursor: pointer;
  border-radius: 3px;
}

button:disabled {
  background-color: #ddd;
  cursor: not-allowed;
}

.error {
  color: red;
  font-size: 12px;
  margin-top: 4px;
}

.signup-link {
  text-align: center;
  margin-top: 20px;
  font-size: 13px;
}

.signup-link a {
  color: #007bff;
  cursor: pointer;
}
