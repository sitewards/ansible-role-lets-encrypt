# Change Log
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/) and this project adheres to 
[Semantic Versioning](http://semver.org/).

## 2.0.0

### changed
- Update to use acme v2, as acme v1 was depricated and removed by letsencrypt
- Use latest ansible version 1.8.0 (changes might be incompatible with older versions of ansible)


## 1.1.1

### changed
- Correcting some typo error (wellknow => wellknown)

### Removed
- Debug comments

## 1.1.0

### Added
- http-01 (wellknown acme-challenge) support.
- SSL crt & key concatenation for HAProxy compatibility.
- Ansible Galaxy file structure (to be used with ansible-galaxy install git@...).

### Changed
- README : adding http-01 support.
- lets_encrypt_directory var is dynamically set (stage or prod URL). 
- main.yml to be used with http-01.

## 1.0.0

### Added

- Everything. This is the initial release.
