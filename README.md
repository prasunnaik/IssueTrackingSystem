import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { Project } from '../models/project';

@Injectable({
  providedIn: 'root'
})
export class ProjectService {

  private apiUrl = 'http://localhost:8082/api/projects';

  constructor(private http: HttpClient) {}

  getProjectsByOwner(ownerId: number): Observable<Project[]> {
    return this.http.get<Project[]>(
      `${this.apiUrl}/owner/${ownerId}`
    );
  }

  getProjectById(projectId: number): Observable<Project> {
    return this.http.get<Project>(
      `${this.apiUrl}/${projectId}`
    );
  }

  deleteProject(projectId: number): Observable<void> {
    return this.http.delete<void>(
      `${this.apiUrl}/${projectId}`
    );
  }
}


import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ProjectService } from '../../services/project.service';
import { Project } from '../../models/project';

@Component({
  selector: 'app-owner-dashboard',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './owner-dashboard.component.html',
  styleUrl: './owner-dashboard.component.css'
})
export class OwnerDashboardComponent implements OnInit {

  projects: Project[] = [];
  errorMessage = '';

  constructor(private projectService: ProjectService) {}

  ngOnInit(): void {
    const userData = localStorage.getItem('user');

    if (!userData) {
      this.errorMessage = 'User not logged in';
      return;
    }

    const user = JSON.parse(userData);

    this.projectService.getProjectsByOwner(user.id).subscribe({
      next: (response) => {
        this.projects = response;
        console.log('Owner projects:', response);
      },
      error: (error) => {
        console.error('Error loading projects:', error);
        this.errorMessage = 'Unable to load projects';
      }
    });
  }
}
