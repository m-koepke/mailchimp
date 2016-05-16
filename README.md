[![Hex Version](http://img.shields.io/hexpm/v/mailchimp.svg)](https://hex.pm/packages/mailchimp)

This is a basic Elixir wrapper for version 3 of the MailChimp API.

## Installation

First, add MailChimp lib to your `mix.exs` dependencies:

```elixir
def deps do
  [{:mailchimp, "~> 0.0.2"}]
end
```

and run `$ mix deps.get`. Now, list the `:mailchimp` application as your
application dependency:

```elixir
def application do
  [applications: [:mailchimp]]
end
```

## Usage

1. Put your API key in your *config.exs* file:

```elixir
config :mailchimp,
  apikey: "your api-us10"
```

2. Start a new process:

    Mailchimp.start_link

### Phoenix and OTP

You can easily utilize Phoenix and OTP to start Mailchimp supervised. Simply add
Mailchimp as a worker process to your Application:

    # lib/my_app.ex
    defmodule MyApp do
      use Application

      def start(_type, _args) do
        # ...
        children = [
          # ...
          worker(Mailchimp, [[name: :mailchimp]])
        ]
        # ...
      end
      # ...
    end

### Getting Account Details

    Mailchimp.get_account_details()

### Getting All Lists

    Mailchimp.get_all_lists()

### Adding a Member to a List

    Mailchimp.add_member("list_id", "e-mail")

### Example of adding a Member to a List with merge_fields:

    def add_to_list(list_id, user) do
      merge_fields = %{
        "FNAME" => user.first_name,
        "LNAME" => user.last_name,
        "COMPANY" => user.organisation,
        "MMERGE4" => %{
          "addr1" => user.address,
          "addr2" => "-",
          "city" => user.city,
          "state" => "-",
          "zip" => to_string(user.postal_code),
          "country" => user.country,
        }
      }

      Mailchimp.add_member(list_id, user.email, merge_fields)
    end

Hint: Merge fields of type "address" require ALL fields to be of type "string". Ensure your zip code is a string.
