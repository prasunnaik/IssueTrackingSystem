<img
  class="profile-image"
  [src]="profileImage || 'assets/default-profile.png'"
  [alt]="ownerName + ' Profile'"
  (error)="profileImage = 'assets/default-profile.png'"
/>

this.ownerName = user.name || 'Project Owner';
this.ownerEmail = user.email || '';
this.profileImage = user.profileImage || '';
