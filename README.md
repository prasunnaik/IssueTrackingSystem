import { Routes } from '@angular/router';

export const routes: Routes = [
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
