import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { Router } from '@angular/router';

import { IssueService } from '../../services/issue.service';
import { ProjectService } from '../../services/project.service';

import { Issue } from '../../models/issue';
import { Project } from '../../models/project';


@Component({
  selector: 'app-owner-dashboard',
  standalone: true,
  imports: [
    CommonModule,
    FormsModule
  ],
  templateUrl: './owner-dashboard.component.html',
  styleUrls: ['./owner-dashboard.component.css']
})
export class OwnerDashboardComponent implements OnInit {

  // =========================================================
  // LOGGED-IN OWNER
  // =========================================================

  ownerId: number = 0;

  ownerName: string = 'Project Owner';

  ownerEmail: string = '';

  profileImage: string = 'assets/default-profile.png';


  // =========================================================
  // SEARCH
  // =========================================================

  searchText: string = '';


  // =========================================================
  // PROJECTS
  // =========================================================

  projects: Project[] = [];

  selectedProject: Project | null = null;


  // =========================================================
  // EDIT PROJECT
  // =========================================================

  editMode: boolean = false;

  editProjectData: Project = {
    projectName: '',
    productOwnerId: 1,
    startDate: '',
    endDate: ''
  };


  // =========================================================
  // ISSUES
  // =========================================================

  issues: Issue[] = [];


  // =========================================================
  // FILTER
  // =========================================================

  selectedFilter: string = 'ALL';


  // =========================================================
  // MESSAGES
  // =========================================================

  errorMessage: string = '';

  successMessage: string = '';


  // =========================================================
  // KANBAN COLUMNS
  // =========================================================

  kanbanColumns = [
    {
      status: 'OPEN',
      title: 'TO DO',
      headerClass: 'open-header'
    },
    {
      status: 'IN_PROGRESS',
      title: 'DEVELOPMENT',
      headerClass: 'progress-header'
    },
    {
      status: 'CLOSED',
      title: 'COMPLETED',
      headerClass: 'closed-header'
    }
  ];


  // =========================================================
  // CONSTRUCTOR
  // =========================================================

  constructor(
    private projectService: ProjectService,
    private issueService: IssueService,
    private router: Router
  ) {}


  // =========================================================
  // INIT
  // =========================================================

  ngOnInit(): void {

    const storedUser = localStorage.getItem('user');

    if (!storedUser) {
      this.router.navigate(['/login']);
      return;
    }

    try {

      const user = JSON.parse(storedUser);

      this.ownerId = Number(user.id);

      this.ownerName =
        user.name || 'Project Owner';

      this.ownerEmail =
        user.email || '';

      this.profileImage =
        user.profileImage ||
        user.profileImageUrl ||
        user.profile ||
        user.image ||
        'assets/default-profile.png';

      this.setProfileImagePath();

    } catch (error) {

      console.error(
        'Failed to read logged-in user:',
        error
      );

      this.router.navigate(['/login']);

      return;
    }

    if (!this.ownerId) {

      this.errorMessage =
        'Logged-in user information is missing.';

      return;
    }

    this.loadProjects();
  }


  // =========================================================
  // PROFILE IMAGE
  // =========================================================

  private setProfileImagePath(): void {

    if (
      this.profileImage &&
      !this.profileImage.startsWith('http') &&
      !this.profileImage.startsWith('https') &&
      !this.profileImage.startsWith('data:image') &&
      !this.profileImage.startsWith('assets/')
    ) {

      this.profileImage =
        'assets/' + this.profileImage;
    }
  }


  handleImageError(): void {

    this.profileImage =
      'assets/default-profile.png';
  }


  // =========================================================
  // LOAD PROJECTS
  // =========================================================

  loadProjects(): void {

    this.errorMessage = '';

    this.projectService
      .getProjectsByOwner(this.ownerId)
      .subscribe({

        next: (data: Project[]) => {

          this.projects = data || [];

          if (!this.projects.length) {

            this.selectedProject = null;
            this.issues = [];

            return;
          }

          this.setSelectedProject();

          this.loadIssues();
        },

        error: (error: any) => {

          console.error(
            'Failed to load projects:',
            error
          );

          this.errorMessage =
            'Failed to load projects.';

          this.projects = [];
          this.selectedProject = null;
          this.issues = [];
        }

      });
  }


  // =========================================================
  // SET SELECTED PROJECT
  // =========================================================

  private setSelectedProject(): void {

    if (!this.selectedProject?.id) {

      this.selectedProject =
        this.projects[0];

      return;
    }

    const existingProject =
      this.projects.find(
        project =>
          project.id === this.selectedProject?.id
      );

    this.selectedProject =
      existingProject || this.projects[0];
  }


  // =========================================================
  // SELECT PROJECT
  // =========================================================

  selectProject(project: Project): void {

    this.selectedProject = project;

    this.selectedFilter = 'ALL';

    this.searchText = '';

    this.issues = [];

    this.errorMessage = '';

    this.editMode = false;

    this.loadIssues();
  }


  // =========================================================
  // LOAD ISSUES
  // =========================================================

  loadIssues(): void {

    if (
      !this.selectedProject?.id
    ) {

      this.issues = [];

      return;
    }

    this.issueService
      .getIssuesByProject(
        this.selectedProject.id
      )
      .subscribe({

        next: (data: Issue[]) => {

          this.issues = data || [];

          this.errorMessage = '';
        },

        error: (error: any) => {

          console.error(
            'Failed to load issues:',
            error
          );

          this.issues = [];

          this.errorMessage =
            'Failed to load issues.';
        }

      });
  }


  // =========================================================
  // REFRESH
  // =========================================================

  refreshDashboard(): void {

    this.editMode = false;

    this.successMessage = '';

    this.searchText = '';

    this.loadProjects();
  }


  // =========================================================
  // NAVIGATION
  // =========================================================

  createProject(): void {

    this.router.navigate([
      '/create-project'
    ]);
  }


  createIssue(): void {

    this.router.navigate([
      '/create-issue'
    ]);
  }


  logout(): void {

    localStorage.removeItem('user');

    this.router.navigate(['/login']);
  }


  // =========================================================
  // EDIT PROJECT
  // =========================================================

  startEditProject(): void {

    if (!this.selectedProject) {

      alert(
        'Please select a project.'
      );

      return;
    }

    this.errorMessage = '';

    this.successMessage = '';

    this.editProjectData = {
      id: this.selectedProject.id,
      projectName:
        this.selectedProject.projectName,
      productOwnerId:
        this.selectedProject.productOwnerId,
      startDate:
        this.selectedProject.startDate,
      endDate:
        this.selectedProject.endDate
    };

    this.editMode = true;
  }


  cancelEditProject(): void {

    this.editMode = false;

    this.errorMessage = '';

    this.successMessage = '';
  }


  // =========================================================
  // SAVE PROJECT
  // =========================================================

  saveProject(): void {

    const project =
      this.editProjectData;

    if (!project.id) {

      this.errorMessage =
        'Project ID is missing.';

      return;
    }

    if (!project.projectName?.trim()) {

      this.errorMessage =
        'Project name is required.';

      return;
    }

    if (!project.productOwnerId) {

      this.errorMessage =
        'Product owner ID is required.';

      return;
    }

    if (!project.startDate) {

      this.errorMessage =
        'Start date is required.';

      return;
    }

    if (!project.endDate) {

      this.errorMessage =
        'End date is required.';

      return;
    }

    if (
      project.endDate <
      project.startDate
    ) {

      this.errorMessage =
        'End date cannot be before start date.';

      return;
    }

    this.errorMessage = '';

    this.successMessage = '';

    this.projectService
      .updateProject(
        project.id,
        project
      )
      .subscribe({

        next: (updatedProject: Project) => {

          this.successMessage =
            'Project updated successfully.';

          this.editMode = false;

          this.selectedProject =
            updatedProject;

          this.loadProjects();
        },

        error: (error: any) => {

          console.error(
            'Failed to update project:',
            error
          );

          this.errorMessage =
            'Failed to update project.';
        }

      });
  }


  // =========================================================
  // DELETE PROJECT
  // =========================================================

  deleteProject(): void {

    if (!this.selectedProject?.id) {

      alert(
        'Please select a project.'
      );

      return;
    }

    const projectName =
      this.selectedProject.projectName;

    const projectId =
      this.selectedProject.id;

    if (
      !window.confirm(
        `Are you sure you want to delete "${projectName}"?`
      )
    ) {
      return;
    }

    this.errorMessage = '';

    this.successMessage = '';

    this.projectService
      .deleteProject(projectId)
      .subscribe({

        next: () => {

          this.successMessage =
            'Project deleted successfully.';

          this.selectedProject = null;

          this.issues = [];

          this.editMode = false;

          this.loadProjects();
        },

        error: (error: any) => {

          console.error(
            'Failed to delete project:',
            error
          );

          this.errorMessage =
            'Failed to delete project.';
        }

      });
  }


  // =========================================================
  // FILTER
  // =========================================================

  filterIssues(filter: string): void {

    this.selectedFilter = filter;
  }


  // =========================================================
  // FILTERED ISSUES
  // =========================================================

  get filteredIssues(): Issue[] {

    let result = this.issues;

    if (this.selectedFilter !== 'ALL') {

      result = result.filter(issue => {

        if (
          this.selectedFilter === 'HIGH' ||
          this.selectedFilter === 'MEDIUM'
        ) {

          return issue.priority ===
            this.selectedFilter;
        }

        return issue.status ===
          this.selectedFilter;
      });
    }

    const search =
      this.searchText.trim().toLowerCase();

    if (search) {

      result = result.filter(issue => {

        const summary =
          issue.summary?.toLowerCase() || '';

        const description =
          issue.description?.toLowerCase() || '';

        return (
          summary.includes(search) ||
          description.includes(search)
        );
      });
    }

    return result;
  }


  // =========================================================
  // BOARD ISSUES
  // =========================================================

  getBoardIssues(status: string): Issue[] {

    return this.filteredIssues.filter(
      issue =>
        issue.status === status
    );
  }


  // =========================================================
  // STATISTICS
  // =========================================================

  getTotalIssues(): number {

    return this.issues.length;
  }


  getOpenIssues(): number {

    return this.countBy('status', 'OPEN');
  }


  getInProgressIssues(): number {

    return this.countBy(
      'status',
      'IN_PROGRESS'
    );
  }


  getClosedIssues(): number {

    return this.countBy(
      'status',
      'CLOSED'
    );
  }


  getHighPriorityIssues(): number {

    return this.countBy(
      'priority',
      'HIGH'
    );
  }


  private countBy(
    property: 'status' | 'priority',
    value: string
  ): number {

    return this.issues.filter(
      issue =>
        issue[property] === value
    ).length;
  }


  // =========================================================
  // UPDATE STATUS
  // =========================================================

  updateStatus(
    issue: Issue,
    event: Event
  ): void {

    if (!issue.id) {

      alert(
        'Issue ID is missing.'
      );

      return;
    }

    const status =
      (event.target as HTMLSelectElement)
        .value;

    this.issueService
      .updateIssueStatus(
        issue.id,
        status
      )
      .subscribe({

        next: (updatedIssue: Issue) => {

          issue.status =
            updatedIssue.status;
        },

        error: (error: any) => {

          console.error(
            'Failed to update issue status:',
            error
          );

          alert(
            'Failed to update issue status.'
          );

          this.loadIssues();
        }

      });
  }


  // =========================================================
  // UPDATE PRIORITY
  // =========================================================

  updatePriority(
    issue: Issue,
    event: Event
  ): void {

    if (!issue.id) {

      alert(
        'Issue ID is missing.'
      );

      return;
    }

    const priority =
      (event.target as HTMLSelectElement)
        .value;

    this.issueService
      .updateIssuePriority(
        issue.id,
        priority
      )
      .subscribe({

        next: (updatedIssue: Issue) => {

          issue.priority =
            updatedIssue.priority;
        },

        error: (error: any) => {

          console.error(
            'Failed to update issue priority:',
            error
          );

          alert(
            'Failed to update issue priority.'
          );

          this.loadIssues();
        }

      });
  }


  // =========================================================
  // UPDATE ASSIGNEE
  // =========================================================

  updateAssignee(
    issue: Issue,
    event: Event
  ): void {

    if (!issue.id) {

      alert(
        'Issue ID is missing.'
      );

      return;
    }

    const assigneeId =
      Number(
        (event.target as HTMLSelectElement)
          .value
      );

    if (!assigneeId) {

      alert(
        'Please select a valid assignee.'
      );

      return;
    }

    this.issueService
      .updateIssueAssignee(
        issue.id,
        assigneeId
      )
      .subscribe({

        next: (updatedIssue: Issue) => {

          issue.assigneeId =
            updatedIssue.assigneeId;
        },

        error: (error: any) => {

          console.error(
            'Failed to update issue assignee:',
            error
          );

          alert(
            'Failed to update issue assignee.'
          );

          this.loadIssues();
        }

      });
  }


  // =========================================================
  // STATUS CLASS
  // =========================================================

  getStatusClass(
    status: string
  ): string {

    return {
      OPEN: 'open',
      IN_PROGRESS: 'progress',
      CLOSED: 'closed'
    }[status] || '';
  }


  // =========================================================
  // PRIORITY CLASS
  // =========================================================

  getPriorityClass(
    priority: string
  ): string {

    return {
      HIGH: 'high',
      MEDIUM: 'medium',
      LOW: 'low'
    }[priority] || '';
  }


  // =========================================================
  // ISSUE DETAILS
  // =========================================================

  openIssueDetails(
    issue: Issue
  ): void {

    if (!issue.id) {

      alert(
        'Issue ID is missing.'
      );

      return;
    }

    this.router.navigate([
      '/issue',
      issue.id
    ]);
  }

}
