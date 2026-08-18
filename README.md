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
