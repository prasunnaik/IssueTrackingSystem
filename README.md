import {
  Component,
  OnInit
} from '@angular/core';

import {
  CommonModule
} from '@angular/common';

import {
  FormsModule
} from '@angular/forms';

import {
  Router
} from '@angular/router';

import {
  IssueService
} from '../../services/issue.service';

import {
  ProjectService
} from '../../services/project.service';

import {
  Issue
} from '../../models/issue';

import {
  Project
} from '../../models/project';


@Component({
  selector: 'app-owner-dashboard',

  standalone: true,

  imports: [
    CommonModule,
    FormsModule
  ],

  templateUrl:
    './owner-dashboard.component.html',

  styleUrls: [
    './owner-dashboard.component.css'
  ]
})


export class OwnerDashboardComponent
  implements OnInit {


  // =========================================================
  // LOGGED-IN OWNER
  // =========================================================

  ownerId: number = 0;

  ownerName: string =
    'Project Owner';

  ownerEmail: string = '';

  profileImage: string = '';


  // =========================================================
  // SEARCH
  // =========================================================

  searchText: string = '';


  // =========================================================
  // PROJECTS
  // =========================================================

  projects: Project[] = [];

  selectedProject:
    Project | null = null;


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
  // ERROR / SUCCESS
  // =========================================================

  errorMessage: string = '';

  successMessage: string = '';



  // =========================================================
  // CONSTRUCTOR
  // =========================================================

  constructor(

    private projectService:
      ProjectService,

    private issueService:
      IssueService,

    private router:
      Router

  ) {}



  // =========================================================
  // INIT
  // =========================================================

  ngOnInit(): void {

    const storedUser =
      localStorage.getItem('user');


    // -------------------------------------------------------
    // USER NOT LOGGED IN
    // -------------------------------------------------------

    if (!storedUser) {

      this.router.navigate([
        '/login'
      ]);

      return;
    }


    // -------------------------------------------------------
    // READ USER
    // -------------------------------------------------------

    try {

      const user =
        JSON.parse(storedUser);


      this.ownerId =
        Number(user.id);


      this.ownerName =
        user.name ||
        'Project Owner';


      this.ownerEmail =
        user.email ||
        '';


      this.profileImage =
        user.profileImage ||
        user.profile ||
        '';


    } catch (error) {

      console.error(
        'Failed to read logged-in user:',
        error
      );

      this.router.navigate([
        '/login'
      ]);

      return;
    }


    // -------------------------------------------------------
    // VALIDATE OWNER
    // -------------------------------------------------------

    if (!this.ownerId) {

      this.errorMessage =
        'Logged-in user information is missing.';

      return;
    }


    // -------------------------------------------------------
    // LOAD PROJECTS
    // -------------------------------------------------------

    this.loadProjects();
  }



  // =========================================================
  // LOAD PROJECTS
  // =========================================================

  loadProjects(): void {

    this.errorMessage = '';


    this.projectService
      .getProjectsByOwner(
        this.ownerId
      )
      .subscribe({

        next: (
          data: Project[]
        ) => {

          this.projects =
            data || [];


          if (
            this.projects.length > 0
          ) {


            // ------------------------------------------------
            // KEEP CURRENT PROJECT SELECTED
            // ------------------------------------------------

            if (
              this.selectedProject &&
              this.selectedProject.id !==
                undefined
            ) {

              const existingProject =
                this.projects.find(
                  project =>
                    project.id ===
                    this.selectedProject?.id
                );


              if (existingProject) {

                this.selectedProject =
                  existingProject;

              } else {

                this.selectedProject =
                  this.projects[0];
              }


            } else {

              this.selectedProject =
                this.projects[0];
            }


            // ------------------------------------------------
            // LOAD ISSUES
            // ------------------------------------------------

            this.loadIssues();


          } else {

            this.selectedProject =
              null;

            this.issues = [];
          }

        },


        error: (
          error: any
        ) => {

          console.error(
            'Failed to load projects:',
            error
          );


          this.errorMessage =
            'Failed to load projects.';


          this.projects = [];

          this.selectedProject =
            null;

          this.issues = [];
        }

      });
  }



  // =========================================================
  // SELECT PROJECT
  // =========================================================

  selectProject(
    project: Project
  ): void {

    this.selectedProject =
      project;


    this.selectedFilter =
      'ALL';


    this.searchText =
      '';


    this.issues = [];


    this.errorMessage = '';


    this.editMode =
      false;


    this.loadIssues();
  }



  // =========================================================
  // LOAD ISSUES
  // =========================================================

  loadIssues(): void {

    if (
      !this.selectedProject ||
      this.selectedProject.id ===
        undefined
    ) {

      this.issues = [];

      return;
    }


    const projectId =
      this.selectedProject.id;


    this.issueService
      .getIssuesByProject(
        projectId
      )
      .subscribe({

        next: (
          data: Issue[]
        ) => {

          this.issues =
            data || [];

          this.errorMessage = '';
        },


        error: (
          error: any
        ) => {

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

    this.editMode =
      false;

    this.successMessage =
      '';

    this.searchText =
      '';

    this.loadProjects();
  }



  // =========================================================
  // CREATE PROJECT
  // =========================================================

  createProject(): void {

    this.router.navigate([
      '/create-project'
    ]);
  }



  // =========================================================
  // CREATE ISSUE
  // =========================================================

  createIssue(): void {

    this.router.navigate([
      '/create-issue'
    ]);
  }



  // =========================================================
  // LOGOUT
  // =========================================================

  logout(): void {

    localStorage.removeItem(
      'user'
    );

    this.router.navigate([
      '/login'
    ]);
  }



  // =========================================================
  // START EDIT PROJECT
  // =========================================================

  startEditProject(): void {

    if (
      !this.selectedProject
    ) {

      alert(
        'Please select a project.'
      );

      return;
    }


    this.errorMessage = '';

    this.successMessage = '';


    this.editProjectData = {

      id:
        this.selectedProject.id,

      projectName:
        this.selectedProject.projectName,

      productOwnerId:
        this.selectedProject.productOwnerId,

      startDate:
        this.selectedProject.startDate,

      endDate:
        this.selectedProject.endDate

    };


    this.editMode =
      true;
  }



  // =========================================================
  // CANCEL EDIT
  // =========================================================

  cancelEditProject(): void {

    this.editMode =
      false;

    this.errorMessage =
      '';

    this.successMessage =
      '';
  }



  // =========================================================
  // SAVE PROJECT
  // =========================================================

  saveProject(): void {

    if (
      !this.editProjectData.id
    ) {

      this.errorMessage =
        'Project ID is missing.';

      return;
    }


    if (
      !this.editProjectData.projectName ||
      !this.editProjectData.projectName.trim()
    ) {

      this.errorMessage =
        'Project name is required.';

      return;
    }


    if (
      !this.editProjectData.productOwnerId
    ) {

      this.errorMessage =
        'Product owner ID is required.';

      return;
    }


    if (
      !this.editProjectData.startDate
    ) {

      this.errorMessage =
        'Start date is required.';

      return;
    }


    if (
      !this.editProjectData.endDate
    ) {

      this.errorMessage =
        'End date is required.';

      return;
    }


    if (
      this.editProjectData.endDate <
      this.editProjectData.startDate
    ) {

      this.errorMessage =
        'End date cannot be before start date.';

      return;
    }


    this.errorMessage =
      '';

    this.successMessage =
      '';


    const projectId =
      this.editProjectData.id;


    this.projectService
      .updateProject(
        projectId,
        this.editProjectData
      )
      .subscribe({

        next: (
          updatedProject: Project
        ) => {

          this.successMessage =
            'Project updated successfully.';


          this.editMode =
            false;


          this.selectedProject =
            updatedProject;


          this.loadProjects();
        },


        error: (
          error: any
        ) => {

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

    if (
      !this.selectedProject ||
      this.selectedProject.id ===
        undefined
    ) {

      alert(
        'Please select a project.'
      );

      return;
    }


    const projectName =
      this.selectedProject.projectName;


    const projectId =
      this.selectedProject.id;


    const confirmed =
      window.confirm(
        `Are you sure you want to delete "${projectName}"?`
      );


    if (!confirmed) {

      return;
    }


    this.errorMessage =
      '';

    this.successMessage =
      '';


    this.projectService
      .deleteProject(
        projectId
      )
      .subscribe({

        next: () => {

          this.successMessage =
            'Project deleted successfully.';


          this.selectedProject =
            null;


          this.issues = [];


          this.editMode =
            false;


          this.loadProjects();
        },


        error: (
          error: any
        ) => {

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

  filterIssues(
    filter: string
  ): void {

    this.selectedFilter =
      filter;
  }



  // =========================================================
  // SEARCH + FILTERED ISSUES
  // =========================================================

  get filteredIssues(): Issue[] {

    let result =
      this.issues;


    // -------------------------------------------------------
    // STATUS / PRIORITY FILTER
    // -------------------------------------------------------

    if (
      this.selectedFilter ===
      'OPEN'
    ) {

      result =
        result.filter(
          issue =>
            issue.status ===
            'OPEN'
        );

    } else if (
      this.selectedFilter ===
      'IN_PROGRESS'
    ) {

      result =
        result.filter(
          issue =>
            issue.status ===
            'IN_PROGRESS'
        );

    } else if (
      this.selectedFilter ===
      'HIGH'
    ) {

      result =
        result.filter(
          issue =>
            issue.priority ===
            'HIGH'
        );

    } else if (
      this.selectedFilter ===
      'MEDIUM'
    ) {

      result =
        result.filter(
          issue =>
            issue.priority ===
            'MEDIUM'
        );

    } else if (
      this.selectedFilter ===
      'CLOSED'
    ) {

      result =
        result.filter(
          issue =>
            issue.status ===
            'CLOSED'
        );
    }


    // -------------------------------------------------------
    // SEARCH
    // -------------------------------------------------------

    const search =
      this.searchText
        .trim()
        .toLowerCase();


    if (search) {

      result =
        result.filter(
          issue => {

            const summary =
              issue.summary
                ?.toLowerCase() ||
              '';


            const description =
              issue.description
                ?.toLowerCase() ||
              '';


            return (
              summary.includes(search) ||
              description.includes(search)
            );
          }
        );
    }


    return result;
  }



  // =========================================================
  // BOARD ISSUES
  // =========================================================

  getBoardIssues(
    status: string
  ): Issue[] {

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

    return this.issues.filter(
      issue =>
        issue.status ===
        'OPEN'
    ).length;
  }



  getHighPriorityIssues(): number {

    return this.issues.filter(
      issue =>
        issue.priority ===
        'HIGH'
    ).length;
  }



  getInProgressIssues(): number {

    return this.issues.filter(
      issue =>
        issue.status ===
        'IN_PROGRESS'
    ).length;
  }



  getClosedIssues(): number {

    return this.issues.filter(
      issue =>
        issue.status ===
        'CLOSED'
    ).length;
  }



  // =========================================================
  // UPDATE STATUS
  // =========================================================

  updateStatus(
    issue: Issue,
    event: Event
  ): void {

    if (
      issue.id === undefined
    ) {

      alert(
        'Issue ID is missing.'
      );

      return;
    }


    const select =
      event.target as
      HTMLSelectElement;


    const status =
      select.value;


    this.issueService
      .updateIssueStatus(
        issue.id,
        status
      )
      .subscribe({

        next: (
          updatedIssue: Issue
        ) => {

          issue.status =
            updatedIssue.status;
        },


        error: (
          error: any
        ) => {

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

    if (
      issue.id === undefined
    ) {

      alert(
        'Issue ID is missing.'
      );

      return;
    }


    const select =
      event.target as
      HTMLSelectElement;


    const priority =
      select.value;


    this.issueService
      .updateIssuePriority(
        issue.id,
        priority
      )
      .subscribe({

        next: (
          updatedIssue: Issue
        ) => {

          issue.priority =
            updatedIssue.priority;
        },


        error: (
          error: any
        ) => {

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

    if (
      issue.id === undefined
    ) {

      alert(
        'Issue ID is missing.'
      );

      return;
    }


    const select =
      event.target as
      HTMLSelectElement;


    const assigneeId =
      Number(select.value);


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

        next: (
          updatedIssue: Issue
        ) => {

          issue.assigneeId =
            updatedIssue.assigneeId;
        },


        error: (
          error: any
        ) => {

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

    if (
      status === 'OPEN'
    ) {

      return 'open';
    }


    if (
      status === 'IN_PROGRESS'
    ) {

      return 'progress';
    }


    if (
      status === 'CLOSED'
    ) {

      return 'closed';
    }


    return '';
  }



  // =========================================================
  // PRIORITY CLASS
  // =========================================================

  getPriorityClass(
    priority: string
  ): string {

    if (
      priority === 'HIGH'
    ) {

      return 'high';
    }


    if (
      priority === 'MEDIUM'
    ) {

      return 'medium';
    }


    if (
      priority === 'LOW'
    ) {

      return 'low';
    }


    return '';
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
