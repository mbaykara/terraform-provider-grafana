---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "grafana_oncall_schedule Resource - terraform-provider-grafana"
subcategory: "OnCall"
description: |-
  HTTP API https://grafana.com/docs/oncall/latest/oncall-api-reference/schedules/
---

# grafana_oncall_schedule (Resource)

* [HTTP API](https://grafana.com/docs/oncall/latest/oncall-api-reference/schedules/)

## Example Usage

```terraform
data "grafana_oncall_slack_channel" "example_slack_channel" {
  name = "example_slack_channel"
}
data "grafana_oncall_user_group" "example_user_group" {
  slack_handle = "example_slack_handle"
}

data "grafana_team" "my_team" {
  name = "my team"
}

data "grafana_oncall_team" "my_team" {
  name = data.grafana_team.my_team.name
}

// ICal based schedule
resource "grafana_oncall_schedule" "example_schedule" {
  name               = "Example Ical Schadule"
  type               = "ical"
  ical_url_primary   = "https://example.com/example_ical.ics"
  ical_url_overrides = "https://example.com/example_overrides_ical.ics"

  // Optional: specify the team to which the schedule belongs
  team_id = data.grafana_oncall_team.my_team.id

  slack {
    channel_id    = data.grafana_oncall_slack_channel.example_slack_channel.slack_id
    user_group_id = data.grafana_oncall_user_group.example_user_group.slack_id
  }
}

// Shift based schedule
resource "grafana_oncall_schedule" "example_schedule" {
  name      = "Example Calendar Schadule"
  type      = "calendar"
  time_zone = "America/New_York"

  // Optional: specify the team to which the schedule belongs
  team_id = data.grafana_oncall_team.my_team.id

  shifts = [
  ]
  ical_url_overrides = "https://example.com/example_overrides_ical.ics"
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `name` (String) The schedule's name.
- `type` (String) The schedule's type. Valid values are `ical`, `calendar`, `web`.

### Optional

- `enable_web_overrides` (Boolean) Enable overrides via web UI (it will ignore ical_url_overrides).
- `ical_url_overrides` (String) The URL of external iCal calendar which override primary events.
- `ical_url_primary` (String) The URL of the external calendar iCal file.
- `shifts` (Set of String) The list of ID's of on-call shifts.
- `slack` (Block List, Max: 1) The Slack-specific settings for a schedule. (see [below for nested schema](#nestedblock--slack))
- `team_id` (String) The ID of the OnCall team (using the `grafana_oncall_team` datasource).
- `time_zone` (String) The schedule's time zone.

### Read-Only

- `id` (String) The ID of this resource.

<a id="nestedblock--slack"></a>
### Nested Schema for `slack`

Optional:

- `channel_id` (String) Slack channel id. Reminder about schedule shifts will be directed to this channel in Slack.
- `user_group_id` (String) Slack user group id. Members of user group will be updated when on-call users change.

## Import

Import is supported using the following syntax:

```shell
terraform import grafana_oncall_schedule.name "{{ id }}"
```
