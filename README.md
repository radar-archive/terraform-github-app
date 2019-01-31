# Terraform Github App

This project listens for PR webhook events in the `https://github.inradar.net/automatoninc/terraform` repository. 
It will download the latest commit of the PR and ensure that the syntax is correct. IT will then post the results 
of `terraform plan` as a comment to the PR.

In the PR you can then comment `!apply` to run `terraform apply`. The app will post the results of `terraform apply`
and in case of a successful apply, merge the PR, otherwise another comment will be posted describing the failure.

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
