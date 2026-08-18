import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';

import { ProjectService } from '../../services/project.service';
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

  // We are not importing Issue because your issue model file
  // is currently not available at ../../models/issue
  issues: any[] = [];

  errorMessage = '';

  constructor(
    private projectService: ProjectService,
    private issueService: IssueService
  ) {}

  ngOnInit(): void {

    const userData = localStorage.getItem('user');

    if (!userData) {
      this.errorMessage = 'User not logged in';
      return;
    }

    const user = JSON.parse(userData);

    this.projectService.getProjectsByOwner(user.id).subscribe({

      next: (response: Project[]) => {

        this.projects = response;

        console.log('Owner projects:', response);

        // Load issues for each project
        this.loadIssues();

      },

      error: (error) => {

        console.error('Error loading projects:', error);

        this.errorMessage = 'Unable to load projects';

      }

    });
  }


  loadIssues(): void {

    this.issues = [];

    if (this.projects.length === 0) {
      return;
    }

    this.projects.forEach((project) => {

      if (project.id !== undefined) {

        this.issueService.getIssuesByProject(project.id).subscribe({

          next: (response) => {

            this.issues = [
              ...this.issues,
              ...response
            ];

            console.log(
              'Project issues:',
              project.id,
              response
            );

          },

          error: (error) => {

            console.error(
              'Error loading issues for project:',
              project.id,
              error
            );

          }

        });

      }

    });

  }


  getOpenIssues(): number {

    return this.issues.filter(
      issue =>
        issue.status &&
        issue.status.toString().toUpperCase() === 'OPEN'
    ).length;

  }


  getHighPriorityIssues(): number {

    return this.issues.filter(
      issue =>
        issue.priority &&
        issue.priority.toString().toUpperCase() === 'HIGH'
    ).length;

  }

}
