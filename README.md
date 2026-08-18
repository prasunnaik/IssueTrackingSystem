@Override
public Issue updateIssuePriority(Long issueId, String priority) {

    Issue issue = issueRepository.findById(issueId)
            .orElseThrow(() ->
                    new RuntimeException("Issue not found: " + issueId));

    issue.setPriority(priority);

    return issueRepository.save(issue);
}
