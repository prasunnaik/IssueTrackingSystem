import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import {
  ReactiveFormsModule,
  FormBuilder,
  FormGroup,
  Validators
} from '@angular/forms';
import { RouterLink } from '@angular/router';

@Component({
  selector: 'app-signup',
  standalone: true,
  imports: [
    CommonModule,
    ReactiveFormsModule,
    RouterLink
  ],
  templateUrl: './signup.component.html',
  styleUrl: './signup.component.css'
})
export class SignupComponent {

  signupForm!: FormGroup;

  constructor(private formBuilder: FormBuilder) {

    this.signupForm = this.formBuilder.group({

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

  }

  signup() {

    if (this.signupForm.invalid) {
      return;
    }

    console.log('Signup details:');
    console.log(this.signupForm.value);
  }
}
