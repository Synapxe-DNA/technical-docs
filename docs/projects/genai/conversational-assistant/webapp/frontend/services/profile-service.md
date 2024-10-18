---
updated: 18 Oct 2024
authors:
  - Alexander Lee
---

# Profile Service

This service is responsible for creating new profiles and managing existing profiles for users.

---

## @label(service) ProfileService

### Attributes

#### @label(private) @label(attr) $profiles

`BehaviorSubject<Profile[]>` stores all profiles retrieved from the database store.

#### @label(private) @label(attr) $currentProfileInUrl

`BehaviorSubject<string>` tracks the current profile through the profile id in the url.

### Methods

#### @label(meth) Set Profile in URL

    setProfileInUrl(id: string)

Description
: Method to update the existing url to the new profile linked to the argument 'id' passed into the function.

#### @label(meth) Create Profile

    createProfile(profile: Profile)

Description
: Method to create a new profile.

#### @label(meth) Get All Profiles

    getProfiles(): BehaviorSubject<Profile[]>

Description
: Method to get all existing profiles in the database store.

#### @label(meth) Get Specific Profiles

    getProfile(profileId: string): BehaviorSubject<Profile | undefined>

Description
: Method to get a specific profiles in the database store. If the profile does not exist, it does not return anything.

#### @label(meth) Update Profile

    updateProfile(updatedProfile: Profile)

Description
: Method to update the profile in updatedProfile.

#### @label(meth) Delete Profile

    deleteProfile(id: string)

Description
: Method to delete a profile by id.
