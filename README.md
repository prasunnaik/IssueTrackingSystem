import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

import { Project } from '../models/project';

@Injectable({
  providedIn: 'root'
})
export class ProjectService {

  private apiUrl = 'http://localhost:8082/api/projects';

  constructor(
    private http: HttpClient
  ) {}

  // Get all projects
  getAllProjects(): Observable<Project[]> {
    return this.http.get<Project[]>(
      this.apiUrl
    );
  }

  // Get project by ID
  getProjectById(
    projectId: number
  ): Observable<Project> {

    return this.http.get<Project>(
      `${this.apiUrl}/${projectId}`
    );
  }

  // Get projects belonging to owner
  getProjectsByOwner(
    ownerId: number
  ): Observable<Project[]> {

    return this.http.get<Project[]>(
      `${this.apiUrl}/owner/${ownerId}`
    );
  }

  // Create project
  createProject(
    project: Project
  ): Observable<Project> {

    return this.http.post<Project>(
      this.apiUrl,
      project
    );
  }

  // Update project
  updateProject(
    projectId: number,
    project: Project
  ): Observable<Project> {

    return this.http.put<Project>(
      `${this.apiUrl}/${projectId}`,
      project
    );
  }

  // Delete project
  deleteProject(
    projectId: number
  ): Observable<void> {

    return this.http.delete<void>(
      `${this.apiUrl}/${projectId}`
    );
  }
}
