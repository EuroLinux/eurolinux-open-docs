# Procedure for migrating EuroLinux to RockyLinux

1. Preparation:
   - Ensure a stable and reliable internet connection throughout the entire migration process. This is critical for downloading scripts and packages.
   - Create a full backup of the system to be migrated.
   - Test the recovery procedure to ensure you can restore the system in case of interruption or errors during migration.
   - It is recommended to run the migration in a session manager, e.g., tmux.

2. Download migration scripts:
   - For EL8:
     ```
     curl -O https://raw.githubusercontent.com/EuroLinux/rocky-tools/feature/vaulted_migration/migrate2rocky/migrate2rocky.sh
     ```
   - For EL9:
     ```
     curl -O https://raw.githubusercontent.com/EuroLinux/rocky-tools/feature/vaulted_migration/migrate2rocky/migrate2rocky9.sh
     ```

3. Migration:
   - Migrating EuroLinux 8 to RockyLinux 8:
     ```
     sudo bash migrate2rocky.sh -r
     ```

   - Migrating EuroLinux 9.4 to RockyLinux 9.4:
     - If RockyLinux has not yet released version 9.5:
       ```
       sudo bash migrate2rocky9.sh -r
       ```
     - If RockyLinux has already released version 9.5 or higher:
       ```
       sudo bash migrate2rocky9.sh -rv 9.4
       ```
