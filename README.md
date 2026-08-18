export interface Issue {
  id?: number;
  projectId?: number;
  assigneeId?: number;

  summary: string;
  description: string;

  status: string;
  priority: string;
  type: string;

  storyPoint: number;

  sprint?: string;
  tags?: string;

  createdDate?: string;
  lastUpdatedDate?: string;
}

import { Component } from '@angular/core';

@Component({
  selector: 'app-issue-details',
  imports: [],
  templateUrl: './issue-details.component.html',
  styleUrl: './issue-details.component.css'
})
export class IssueDetailsComponent {

}

<p>issue-details works!</p>

import { Component } from '@angular/core';

@Component({
  selector: 'app-edit-issue',
  imports: [],
  templateUrl: './edit-issue.component.html',
  styleUrl: './edit-issue.component.css'
})
export class EditIssueComponent {

}

<p>edit-issue works!</p>

import { Routes } from '@angular/router';
import { CreateIssueComponent } from './components/create-issue/create-issue.component';

export const routes: Routes = [
  {
    path: 'create-issue',
    component: CreateIssueComponent
  },
  {
    path: '',
    redirectTo: 'login',
    pathMatch: 'full'
  },
  {
    path: 'login',
    loadComponent: () =>
      import('./components/login/login.component')
        .then(m => m.LoginComponent)
  },
  {
    path: 'signup',
    loadComponent: () =>
      import('./components/signup/signup.component')
        .then(m => m.SignupComponent)
  },
  {
    path: 'owner-dashboard',
    loadComponent: () =>
      import('./components/owner-dashboard/owner-dashboard.component')
        .then(m => m.OwnerDashboardComponent)
  },
  {
    path: 'create-project',
    loadComponent: () =>
      import('./components/create-project/create-project.component')
        .then(m => m.CreateProjectComponent)
  },
  {
    path: 'create-issue',
    loadComponent: () =>
      import('./components/create-issue/create-issue.component')
        .then(m => m.CreateIssueComponent)
  },
  {
    path: 'issue/:id',
    loadComponent: () =>
      import('./components/issue-details/issue-details.component')
        .then(m => m.IssueDetailsComponent)
  },
  {
    path: 'edit-issue/:id',
    loadComponent: () =>
      import('./components/edit-issue/edit-issue.component')
        .then(m => m.EditIssueComponent)
  },
  {
    path: 'assignee-dashboard',
    loadComponent: () =>
      import('./components/assignee-dashboard/assignee-dashboard.component')
        .then(m => m.AssigneeDashboardComponent)
  },
  {
    path: 'assignee-issue/:id',
    loadComponent: () =>
      import('./components/assignee-issue-details/assignee-issue-details.component')
        .then(m => m.AssigneeIssueDetailsComponent)
  }
];
