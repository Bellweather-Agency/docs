# Local Development and WP Engine Deployment

## General Principles
- **code moves up; databases move down**. It is very rare for it to be necessary to overwrite a higher environment's database with that of a lower environment.
  The vast majority of the time you will only need to use the `Export` button here on higher environments and `Import Database` on local <img width="1318" height="747" alt="Screenshot 2026-08-24 at 4 25 29 PM" src="https://github.com/user-attachments/assets/7c4dad68-728a-4799-b694-7835e2822c0d" />

- Similarly you will almost always be **Pulling** from production to staging in WP Engine when data needs to be synced there.

  <img width="857" height="568" alt="Screenshot 2026-08-24 at 4 30 13 PM" src="https://github.com/user-attachments/assets/55b724f7-3778-4c46-825d-5916656ee5e3" />

## Getting an Existing Site Running Locally

1. Confirm that you have account access for the site itself, WP Engine, GitHub, and WP Migrate. Ping a Web Director or Senior Developer if you need the WP Migrate license key. 
2. On the live site admin panel, open **Tools → WP Migrate → Export**.
3. Export the database and media uploads.
4. Create a blank Local site with PHP and Wordpress versions that match the production site.
5. Temporarily move `wp-config.php` outside `Local Sites/SITENAME/app/public/`, then empty that directory.
6. Git clone the project's repository into the `Local Sites/SITENAME/app/public/` directory
7. Move the Local `wp-config.php` back into `app/public/`.
8. Install dependencies and run the project build.
9. Visit the local site's /wp-admin panel, add the WP Migrate plugin, and import the database, making sure URLs are find/replaced appropriately.
10. Overwrite the local site's `/wp-content/uploads` directory with the exported media uploads from the production site.
11. Ensure the appropriate plugins and theme are activated.
12. Confirm that .gitignore excludes database exports and wp-content/uploads/. These are typically ignored by default, but never force-add them or commit production data to Git.


## Deploying Changes to a Site via WP Engine GitPush
### Deploying to a Site for the First time
[Follow these instructions to add an SSH key for WP Engine GitPush](https://wpengine.com/support/git/#toc-add-ssh-key-to-user-portal)

- Visit `https://my.wpengine.com/installs/SITENAME/git_push`, for both production and staging, replacing SITENAME with whichever site you're working on.
- Copy the remote URLs from there to add to your local git setup. 
- Add the WP Engine environments as remotes with `git remote add <remote_name> <remote_url>` and verify with `git remote -v`
- As an example, when working on the OCM site the result of the command should look something like this--although the exact `remote_name` is up to you
<img width="452" height="124" alt="Screenshot 2026-08-24 at 4 43 14 PM" src="https://github.com/user-attachments/assets/116f4833-6c78-46dc-98eb-39bedb6a7b1a" />

### Deploying after merging a PR
1. Once your PR has been merged into main, make sure you're on your local main branch and that it has been fetched to be up to date with origin.
2. Push to staging by running `git push staging HEAD` or whatever you've named your staging remote.
3. Verify your changes on staging yourself and with relevant stakeholders. For visual changes be sure to test them at a variety of viewports. If a change requires client approval do not deploy to production before they have seen it.
4. Check the browser console and WP Engine errors to be sure no errors have been introduced.
5. Check all site functionality that could be affected by your changes.
6. When your changes are ready for production, follow the same steps but be sure to create WP Engine backup beforehand at `https://my.wpengine.com/installs/SITENAME/backup_points`.
7. If a site ever goes down upon deployment, immediately `@here` mention the relevant slack channel and directors/senior developers and wait for their input. Almost always the next step will be simply reverting the code changes via GitPush or restoring from the previous WP Engine backup point and then diagnosing issues on staging/locally/via logs.
8. WP Engine's full cache setup includes CDN edge caching of full pages as well as database query object caches. Because of this it may take a few minutes to see your changes on the live site.

Other Resources: https://wpengine.com/support/development-workflow-best-practices/

## Creating a New Site
There are several ways to approach this, TBD while we potentially work out a site template repo or similar. One general method:

1. Create a new WordPress site in Local.
2. Match the PHP and WordPress versions expected in production.
3. Create a **private** repository in the Bellweather GitHub organization.
4. Add the appropriate `.gitignore`, `README.md`, and project files.
5. Initialize a repo in the `Local Sites/SITENAME/app/public/` directory and push the Local site to the Github repository.
6. Create staging and production environments in WP Engine.
7. Document the following in the project README:
   - Local setup instructions
   - Dependency and build commands
   - Required plugins and integrations
   - Environment-specific configuration
