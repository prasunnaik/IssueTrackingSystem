import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { Router } from '@angular/router';

import { IssueService } from '../../services/issue.service';
import { ProjectService } from '../../services/project.service';

import { Issue } from '../../models/issue';
import { Project } from '../../models/project';

@Component({
  selector: 'app-create-issue',
  standalone: true,
  imports: [
    CommonModule,
    FormsModule
  ],
  templateUrl: './create-issue.component.html',
  styleUrls: ['./create-issue.component.css']
})
export class CreateIssueComponent implements OnInit {

  ownerId: number = 1;

  projects: Project[] = [];

  errorMessage: string = '';

  successMessage: string = '';

  loading: boolean = false;

  issue: Issue = {
    summary: '',
    description: '',
    status: 'OPEN',
    priority: 'MEDIUM',
    type: 'TASK',
    assigneeId: 1,
    storyPoint: 1,
    projectId: undefined,
    sprint: '',
    tags: ''
  } as Issue;


  constructor(
    private issueService: IssueService,
    private projectService: ProjectService,
    private router: Router
  ) {}


  ngOnInit(): void {

    this.loadProjects();
  }


  loadProjects(): void {

    this.projectService
      .getProjectsByOwner(this.ownerId)
      .subscribe({

        next: (data: Project[]) => {

          this.projects = data || [];

          if (
            this.projects.length > 0 &&
            this.issue.projectId === undefined
          ) {

            this.issue.projectId =
              this.projects[0].id;
          }
        },

        error: (error: any) => {

          console.error(
            'Failed to load projects:',
            error
          );

          this.errorMessage =
            'Failed to load projects.';
        }
      });
  }


  createIssue(): void {

    this.errorMessage = '';

    this.successMessage = '';


    // Summary validation

    if (
      !this.issue.summary ||
      !this.issue.summary.trim()
    ) {

      this.errorMessage =
        'Issue summary is required.';

      return;
    }


    // Description validation

    if (
      !this.issue.description ||
      !this.issue.description.trim()
    ) {

      this.errorMessage =
        'Issue description is required.';

      return;
    }


    // Project validation

    if (
      this.issue.projectId === undefined ||
      this.issue.projectId === null
    ) {

      this.errorMessage =
        'Please select a project.';

      return;
    }


    // Assignee validation

    if (
      !this.issue.assigneeId
    ) {

      this.errorMessage =
        'Please select an assignee.';

      return;
    }


    // Story point validation

    if (
      !this.issue.storyPoint ||
      this.issue.storyPoint <= 0
    ) {

      this.errorMessage =
        'Story points must be greater than 0.';

      return;
    }


    this.loading = true;


    this.issueService
      .createIssue(this.issue)
      .subscribe({

        next: (createdIssue: Issue) => {

          this.loading = false;

          this.successMessage =
            'Issue created successfully.';

          console.log(
            'Created issue:',
            createdIssue
          );


          // Reset form

          this.issue = {
            summary: '',
            description: '',
            status: 'OPEN',
            priority: 'MEDIUM',
            type: 'TASK',
            assigneeId: 1,
            storyPoint: 1,
            projectId:
              this.projects.length > 0
                ? this.projects[0].id
                : undefined,
            sprint: '',
            tags: ''
          } as Issue;
        },


        error: (error: any) => {

          this.loading = false;

          console.error(
            'Failed to create issue:',
            error
          );

          this.errorMessage =
            'Failed to create issue.';
        }
      });
  }


  goBack(): void {

    this.router.navigate([
      '/owner-dashboard'
    ]);
  }
}
