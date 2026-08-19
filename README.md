import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { Router, RouterLink } from '@angular/router';
import { UserService } from '../../services/user.service';

@Component({
  selector: 'app-login',
  standalone: true,
  imports: [
    CommonModule,
    FormsModule,
    RouterLink
  ],
  templateUrl: './login.component.html',
  styleUrl: './login.component.css'
})
export class LoginComponent {

  user = {
    email: '',
    password: '',
    role: ''
  };

  errorMessage = '';

  constructor(
    private userService: UserService,
    private router: Router
  ) {}

  login(): void {

    this.errorMessage = '';

    this.userService.login(this.user).subscribe({

      next: (response) => {

        console.log('Login successful:', response);

        // Store complete logged-in user
        localStorage.setItem(
          'user',
          JSON.stringify(response)
        );

        // Navigate according to backend role
        if (response.role === 'Project_Owner') {

          this.router.navigate([
            '/owner-dashboard'
          ]);

        } else if (response.role === 'Assignee') {

          this.router.navigate([
            '/assignee-dashboard'
          ]);

        } else {

          this.errorMessage =
            'Invalid user role.';
        }
      },

      error: (error) => {

        console.error(
          'Login failed:',
          error
        );

        this.errorMessage =
          'Invalid email or password.';
      }
    });
  }
}
<div class="login-page">

  <div class="login-box">

    <h2>Issue Tracking System</h2>

    <h3>Login</h3>

    <form #loginForm="ngForm" (ngSubmit)="login()">

      <!-- Email -->
      <div class="form-group">

        <label>Email address</label>

        <input
          type="email"
          name="email"
          [(ngModel)]="user.email"
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
          [(ngModel)]="user.password"
          required
          #passwordField="ngModel"
          placeholder="Enter password">

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
          [(ngModel)]="user.role"
          required
          #roleField="ngModel">

          <option value="">
            Select your role
          </option>

          <option value="Project_Owner">
            Project Owner
          </option>

          <option value="Assignee">
            Assignee
          </option>

        </select>

        <div
          class="error"
          *ngIf="roleField.invalid && roleField.touched">

          Role is required

        </div>

      </div>


      <!-- Backend error -->
      <div
        class="server-error"
        *ngIf="errorMessage">

        {{ errorMessage }}

      </div>


      <!-- Login button -->
      <button
        type="submit"
        [disabled]="loginForm.invalid">

        Login

      </button>

    </form>


    <!-- Signup link -->
    <div class="signup-link">

      Don't have an account?

      <a routerLink="/signup">
        Signup
      </a>

    </div>

  </div>

</div>
