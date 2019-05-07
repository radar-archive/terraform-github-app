# Terraform Github App

This project listens for PR webhook events in the `https://github.inradar.net/automatoninc/terraform` repository. 
It will download the latest commit of the PR and ensure that the syntax is correct. IT will then post the results 
of `terraform plan` as a comment to the PR.

In the PR you can then comment `!apply` to run `terraform apply`. The app will post the results of `terraform apply`
and in case of a successful apply, merge the PR, otherwise another comment will be posted describing the failure.

## Design & Flow

1. User alters infrastructure within the terraform code, and creates a PR in the `terraform` repository.
2. Github Enterprise sends a webhook containing the [PullRequestEvent](https://developer.github.com/v3/activity/events/types/#pullrequestevent)
   - whose action is `opened` - to the Terraform Github App (hosted on a machine in azure) 
3. The Terraform Github App Server pulls the PR from Github
4. `terraform validate` is run on the pulled code. 
  a. In the case of an error, the github app comments on the PR with the error.
  b. In the case of success, the github app runs a `terraform plan`
    1. in the case of success, the github app approves a PR review with the result `:shipit`.
    2. in the case of error, the github app creates a "changes requested" PR review with the error message as the body.
5. [TODO] User sees that `terraform plan` is as expected, and comments `terraform apply` or `!apply` 
   a. If the result is not as expected, the flow starts again, except the Github App will listen for `edited`
6. [TODO] Github Enterprise sends a webhook containing the [PullRequestReviewCommentEvent](https://developer.github.com/v3/activity/events/types/#pullrequestreviewcommentevent)
   - whose action is `created` - to the Terraform Github App
7. [TODO] The terraform Github App performs the `terraform apply`
  a. In the case of success, the github app comments on the PR with the results, and then merges the PR
  b. In the case of error, the github app comments on the PR with the results.

## Deployment

This app should exist on a physical computer that lives either in the NYC or SD office, 
with the hostname `terraform-github-app.[eastus|westus]`.`inradar.net`. Since this is an app which directly deals with infrastructure,
this host should exist _outside_ of terraform, and be initialized via scripts.

## Setting up your dev environment and Testing

Please follow the instructions on [this guide](https://developer.github.com/enterprise/2.15/apps/quickstart-guides/setting-up-your-development-environment/)
to set up your development environment to test this github app.

There are a few more steps in order to test using terraform:

<TODO>


## Install

To run the code, make sure you have [Bundler](http://gembundler.com/) installed; then enter `bundle install` on the command line.

## Set environment variables

1. Create a copy of the `.env-example` file called `.env`.
2. Add your GitHub App's private key, app ID, and webhook secret to the `.env` file.

## Run the server

1. Run `ruby template_server.rb` or `ruby server.rb` on the command line.
1. View the default Sinatra app at `localhost:3000`.
