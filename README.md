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

  login() {

    this.errorMessage = '';

    this.userService.login(this.user).subscribe({

      next: (response) => {

        console.log('Login successful');
        console.log(response);

        localStorage.setItem(
          'user',
          JSON.stringify(response)
        );

        if (response.role === 'Project_Owner') {
          this.router.navigate(['/owner-dashboard']);
        } else if (response.role === 'Assignee') {
          this.router.navigate(['/assignee-dashboard']);
        }

      },

      error: (error) => {

        console.log(error);

        this.errorMessage = 'Invalid email or password';

      }

    });
  }
}
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import {
  ReactiveFormsModule,
  FormBuilder,
  FormGroup,
  Validators
} from '@angular/forms';
import { RouterLink } from '@angular/router';
import { UserService } from '../../services/user.service';

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

  constructor(private formBuilder: FormBuilder, private userService: UserService) {

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

  this.userService.signup(this.signupForm.value).subscribe({
    next: (response) => {
      console.log('Signup successful:', response);
      alert('Signup successful!');
    },
    error: (error) => {
      console.error('Signup failed:', error);
      alert('Signup failed!');
    }
  });
}

}

import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { User } from '../models/user';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class UserService {

  private apiUrl = 'http://localhost:8081/api/users';

  constructor(private http: HttpClient) {}

  signup(user: User): Observable<User> {
    return this.http.post<User>(this.apiUrl, user);
  }

  login(user: {
  email: string;
  password: string;
  role: string;
}): Observable<User> {

  return this.http.post<User>(
    this.apiUrl + '/login',
    user
  );
}


  getAllUsers(): Observable<User[]> {
    return this.http.get<User[]>(this.apiUrl);
  }

  getUserById(id: number): Observable<User> {
    return this.http.get<User>(
      this.apiUrl + '/' + id
    );
  }

  getUserIssues(id: number): Observable<any[]> {
    return this.http.get<any[]>(
      this.apiUrl + '/' + id + '/issues'
    );
  }
}


package com.its.user.controller;

import java.util.List;
import java.util.Map;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.its.user.client.IssueClient;
import com.its.user.model.User;
import com.its.user.service.UserService;

import jakarta.validation.Valid;

@CrossOrigin(origins = "http://localhost:4300")
@RestController
@RequestMapping("/api/users")
public class UserController {

    private final UserService userService;
    private final IssueClient issueClient;

    public UserController(UserService userService, IssueClient issueClient) {
        this.userService = userService;
        this.issueClient = issueClient;
    }

    // Get all users
    @GetMapping
    public ResponseEntity<List<User>> getAllUsers() {

        return new ResponseEntity<>(
                userService.getAllUsers(),
                HttpStatus.OK
        );
    }

    // Register a new user
    @PostMapping
    public ResponseEntity<User> registerUser(
            @Valid @RequestBody User user) {

        return new ResponseEntity<>(
                userService.registerUser(user),
                HttpStatus.CREATED
        );
    }

    // Login
    @PostMapping("/login")
    public ResponseEntity<User> login(
            @RequestBody User user) {

        return new ResponseEntity<>(
                userService.login(
                        user.getEmail(),
                        user.getPassword()
                ),
                HttpStatus.OK
        );
    }

    // Get user by ID
    @GetMapping("/{userId}")
    public ResponseEntity<User> getUserById(
            @PathVariable Long userId) {

        return new ResponseEntity<>(
                userService.getUserById(userId),
                HttpStatus.OK
        );
    }

    // Get issues assigned to a user by user ID
    // Inter-service communication: User Service -> Issue Service
    @GetMapping("/{userId}/issues")
    public ResponseEntity<List<Map<String, Object>>> getUserIssues(
            @PathVariable Long userId) {

        return new ResponseEntity<>(
                issueClient.getIssuesByAssignee(userId),
                HttpStatus.OK
        );
    }

    // Get issues assigned to a user by username
    // Inter-service communication: User Service -> Issue Service
    @GetMapping("/username/{username}/issues")
    public ResponseEntity<List<Map<String, Object>>> getUserIssuesByUsername(
            @PathVariable String username) {

        User user = userService.getUserByName(username);

        return new ResponseEntity<>(
                issueClient.getIssuesByAssignee(user.getId()),
                HttpStatus.OK
        );
    }
}


package com.its.user.model;

import com.fasterxml.jackson.annotation.JsonProperty;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.Table;
import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.NotBlank;

@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @NotBlank(message = "Name is required")
    private String name;

    @NotBlank(message = "Email is required")
    @Email(message = "Please enter a valid email")
    private String email;

    @NotBlank(message = "Password is required")
    @JsonProperty(access = JsonProperty.Access.WRITE_ONLY)
    private String password;

    private String profileImage;

    @NotBlank(message = "Role is required")
    private String role;

    public User() {
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getProfileImage() {
        return profileImage;
    }

    public void setProfileImage(String profileImage) {
        this.profileImage = profileImage;
    }

    public String getRole() {
        return role;
    }

    public void setRole(String role) {
        this.role = role;
    }
}
package com.its.user.repository;

import org.springframework.data.jpa.repository.JpaRepository;

import com.its.user.model.User;

public interface UserRepository extends JpaRepository<User, Long> {

    User findByEmail(String email);

    User findByName(String name);
}
package com.its.user.service;

import java.util.List;

import com.its.user.model.User;

public interface UserService {

    User registerUser(User user);

    User login(String email, String password);

    User getUserById(Long id);

    User getUserByName(String name);

    List<User> getAllUsers();
}
package com.its.user.service;

import java.util.List;

import org.springframework.stereotype.Service;

import com.its.user.exception.InvalidCredentialsException;
import com.its.user.exception.UserNotFoundException;
import com.its.user.model.User;
import com.its.user.repository.UserRepository;

@Service
public class UserServiceImpl implements UserService {

    private final UserRepository userRepository;

    public UserServiceImpl(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    @Override
    public User registerUser(User user) {
        return userRepository.save(user);
    }

    @Override
    public User login(String email, String password) {

        User user = userRepository.findByEmail(email);

        if (user == null || !user.getPassword().equals(password)) {
            throw new InvalidCredentialsException("Invalid email or password");
        }

        return user;
    }

    @Override
    public User getUserById(Long id) {

        return userRepository.findById(id)
                .orElseThrow(() ->
                    new UserNotFoundException(
                        "User not found with id: " + id
                    )
                );
    }

    @Override
    public User getUserByName(String name) {

        User user = userRepository.findByName(name);

        if (user == null) {
            throw new UserNotFoundException(
                "User not found with name: " + name
            );
        }

        return user;
    }

    @Override
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
}
