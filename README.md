<span>
  <strong>Priority:</strong>

  <select
    [value]="issue.priority"
    (change)="updatePriority(
      issue,
      $any($event.target).value
    )">

    <option value="HIGH">
      HIGH
    </option>

    <option value="MEDIUM">
      MEDIUM
    </option>

    <option value="LOW">
      LOW
    </option>

  </select>

</span>
