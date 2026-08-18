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
      next: (response) => {

        this.projects = response;

        console.log('Owner projects:', response);

        if (response.length > 0 && response[0].id !== undefined) {

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
