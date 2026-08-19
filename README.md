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
