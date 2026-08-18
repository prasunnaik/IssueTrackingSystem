import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ProjectsService } from '../../services/project.service';
import { Project } from '../../models/project';
import { IssueService } from '../../services/issue.service';

@Component({
  selector: 'app-owner-dashboard',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './owner-dashboard.component.html',
  styleUrl: './owner-dashboard.component.css'
})
export class OwnerDashboardComponent implements OnInit {

  projects: Project[] = [];
  issues: any[] = [];
  errorMessage = '';

  constructor(
    private projectService: ProjectsService,
    private issueService: IssueService
  ) {}

  ngOnInit(): void {

    const userData = localStorage.getItem('user');

    if (!userData) {
      this.errorMessage = 'User not logged in';
      return;
    }

    const user = JSON.parse(userData);

    // Get projects for the logged-in owner
    this.projectService.getProjectsByOwner(user.id).subscribe({
      next: (response) => {

        this.projects = response;

        console.log('Owner projects:', response);

        // Get issues for the first project
        if (response.length > 0) {

          const projectId = response[0].id;

          this.issueService.getIssuesByProject(projectId).subscribe({
            next: (issues) => {

              this.issues = issues;

              console.log('Project issues:', issues);

            },
            error: (error) => {

              console.error('Error loading issues:', error);

              this.errorMessage = 'Unable to load issues';

            }
          });

        }

      },

      error: (error) => {

        console.error('Error loading projects:', error);

        this.errorMessage = 'Unable to load projects';

      }
    });
  }
}


import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class IssueService {

  private apiUrl = 'http://localhost:8083/api/issues';

  constructor(private http: HttpClient) {}

  getIssuesByProject(projectId: number): Observable<any[]> {
    return this.http.get<any[]>(
      `${this.apiUrl}/project/${projectId}`
    );
  }
}
