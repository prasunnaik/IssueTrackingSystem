import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ReactiveFormsModule, FormBuilder, Validators } from '@angular/forms';
import { RouterLink } from '@angular/router';

@Component({
  selector: 'app-signup',
  standalone: true,
  imports: [CommonModule, ReactiveFormsModule, RouterLink],
  templateUrl: './signup.component.html',
  styleUrl: './signup.component.css'
})
export class SignupComponent {

  signupForm = this.formBuilder.group({
    name: ['', Validators.required],

    email: ['', [
      Validators.required,
      Validators.email
    ]],

    password: ['', Validators.required],

    profile: ['', [
      Validators.required,
      Validators.pattern('https?://.+')
    ]],

    role: ['', Validators.required]
  });

  constructor(private formBuilder: FormBuilder) {}

  signup() {

    if (this.signupForm.invalid) {
      return;
    }

    console.log('Signup details:');
    console.log(this.signupForm.value);
  }
}


<div class="signup-page">

  <div class="signup-box">

    <div class="title">
      Issue Tracking System
    </div>

    <h2>Signup</h2>

    <form
      [formGroup]="signupForm"
      (ngSubmit)="signup()">

      <!-- Name -->
      <div class="form-group">

        <label>Name</label>

        <input
          type="text"
          formControlName="name"
          placeholder="Enter name">

        <div
          class="error"
          *ngIf="
            signupForm.get('name')?.invalid &&
            signupForm.get('name')?.touched
          ">

          Name is required

        </div>

      </div>


      <!-- Email -->
      <div class="form-group">

        <label>Email address</label>

        <input
          type="email"
          formControlName="email"
          placeholder="Enter email">

        <div
          class="error"
          *ngIf="
            signupForm.get('email')?.invalid &&
            signupForm.get('email')?.touched
          ">

          <span
            *ngIf="signupForm.get('email')?.errors?.['required']">
            Email is required
          </span>

          <span
            *ngIf="signupForm.get('email')?.errors?.['email']">
            Enter a valid email address
          </span>

        </div>

      </div>


      <!-- Password -->
      <div class="form-group">

        <label>Password</label>

        <input
          type="password"
          formControlName="password"
          placeholder="Password">

        <div
          class="error"
          *ngIf="
            signupForm.get('password')?.invalid &&
            signupForm.get('password')?.touched
          ">

          Password is required

        </div>

      </div>


      <!-- Profile -->
      <div class="form-group">

        <label>Profile image URL</label>

        <input
          type="text"
          formControlName="profile"
          placeholder="Provide profile image URL">

        <div
          class="error"
          *ngIf="
            signupForm.get('profile')?.invalid &&
            signupForm.get('profile')?.touched
          ">

          <span
            *ngIf="signupForm.get('profile')?.errors?.['required']">
            Profile image URL is required
          </span>

          <span
            *ngIf="signupForm.get('profile')?.errors?.['pattern']">
            Enter a valid image URL
          </span>

        </div>

      </div>


      <!-- Role -->
      <div class="form-group">

        <label>Role</label>

        <select formControlName="role">

          <option value="">
            Select your role
          </option>

          <option value="PROJECT_OWNER">
            Project Owner
          </option>

          <option value="ASSIGNEE">
            Assignee
          </option>

        </select>

        <div
          class="error"
          *ngIf="
            signupForm.get('role')?.invalid &&
            signupForm.get('role')?.touched
          ">

          Role is required

        </div>

      </div>


      <!-- Button -->
      <button
        type="submit"
        [disabled]="signupForm.invalid">

        Signup

      </button>

    </form>


    <!-- Login link -->
    <div class="login-link">

      Already have an account?

      <a routerLink="/login">
        Login
      </a>

    </div>

  </div>

</div>


.signup-page {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #f5f5f5;
}

.signup-box {
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

.login-link {
  text-align: center;
  margin-top: 20px;
  font-size: 13px;
}

.login-link a {
  color: #007bff;
}
