# M3 - `README.md` Example

<br>

# Resortzy

<br>

## Description

Resortzy is a website for resort management. This simplifies the cottage bookings, manging the availability of cottages that meets customer satifaction.

## User Stories

- **404:** As an anon/user I can see a 404 page if I try to reach a page that does not exist so that I know it's my fault
- **Signup:** As a customer, I can sign up in the website so that I can book the cottages based on the availability.
- **Login:** As a customer/manager/admin I can login to the platform so that I can access the particular website features.
- **Logout:** As a customer/manager/admin I can logout from the website so that one else can use it.
- **View Bookings** As a manager I can view all the bookings on daily basis (BACKLOG).
- **Manage membership** As a customer I can choose/change the membership information using the website.
- **Edit profile** As a manager/admin/customer I can edit my profile information using the website.
- **Check availability** As a customer I can check the availability of the cottages on particular dates for booking using the website.
- **Book cottages** As a customer I can book/cancel the cottages on particular dates for booking using the website.

## Backlog

- The website should manage the availability automatically based on system date.
- As a customer I can view my past/present bookings using the website.
- As a manager I can view all the customers information using the website.
- As an admin I can manage all the managers informationn using the website.
- As a manager I can add/edit/delete cottages using the website.
- As a manager I can view all the cottages.

<br>

# Client / Frontend

## React Router Routes (React App)

| Path                      | Component                 | Permissions                | Behavior                                                        |
| ------------------------- | ------------------------- | -------------------------- | --------------------------------------------------------------- |
| `/`                       | HomePage                  | public `<Route>`           | Home page                                                       |
| `/signup`                 | SignupPage                | anon only `<AnonRoute>`    | Signup form, link to login, navigate to homepage after signup   |
| `/login`                  | LoginPage                 | anon only `<AnonRoute>`    | Login form, link to signup, navigate to homepage after login    |
| `/edit-profile`           | EditPage                  | user only `<PrivateRoute>` | Edit profile form,link to Logout and other privileges           |
| `/admin/allmanagers`      | ListManagersPage          | user only `<PrivateRoute>` | Lists all the managers registered to the website                |
| `/admin/addManager`       | AddManagerPage            | user only `<PrivateRoute>` | Add Manager form,to add managers                                |
| `/manager/allcottages`    | ListCottagesPage          | user only `<PrivateRoute>` | Lists all the cottages in the resort                            |
| `/manager/edit-cottage`   | EditCottagesPage          | user only `<PrivateRoute>` | Edit page to edit the cottage information                       |
| `/manager/add-cottage`    | AddCottagesPage           | user only `<PrivateRoute>` | Add cottage form to add a new cottage information to the resort |
| `/manager/allbookings`    | ListBookingsPage          | user only `<PrivateRoute>` | List out all the bookings on daily basis                        |
| `/manager/allcustomers`   | ListCustomerssPage        | user only `<PrivateRoute>` | List out all the customers registered to the resort             |
| `/customer/membership`    | MembershipPage            | user only `<PrivateRoute>` | customer can choose the type of membership                      |
| `/customer/searchpage`    | CustomerHomePage          | user only `<PrivateRoute>` | Customer can check the availability of cottages based on dates  |
| `/customer/bookingpage`   | CustomerBookingPage       | user only `<PrivateRoute>` | Customer can book the cottages                                  |
| `/customer/bookingresult` | CustomerBookingResultPage | user only `<PrivateRoute>` | Booking confirm psge                                            |
| `/customer/mybookings`    | MyBookingPage             | user only `<PrivateRoute>` | List out all bookings of customer                               |
| `/customer/mymembership`  | MyMembershipPage          | user only `<PrivateRoute>` | List out the details of membership                              |

## Components

- HomePage

- SignupPage

- LoginPage

- EditPage

- AddManagerPage

- CustomerHomePage

- CustomerBookingPage

- CustomerBookingResultPage

- ListBookingsPage

- MyMembershipPage

- Footer

## Services

- Auth Service
  - auth.login(user)
  - auth.signup(user)
  - auth.logout()
  - auth.validate()
- Bookings Service
  - bookings.list()
  - bookings.add(id)
  - bookings.delete(id)
- Cottages Service
  - bookings.list()
  - bookings.add(id)
  - bookings.edit(id)
  - bookings.delete(id)
  - bookings.search()
- User Service
  - User.edit(id)
  - User.list()
- Membership Service
  - User.list
    <br>

# Server / Backend

## Models

User model

```javascript
{
  username: {type: String, required: true},
  email: {type: String, required: true, unique: true},
  password: {type: String, required: true},
  address: {type: String, required: true},
  phone: {type: Number, required: true},
  usertype:{type: String, required: true, enum:["admin","manager","customer"]},
  membership:{type: Schema.Types.ObjectId,ref:'Membership'},
  bookings:{[type: Schema.Types.ObjectId,ref:'Booking']},
}
```

Booking model

```javascript
 {
   bookingDate: {type: Date, required: true},
   roomId:{type: Schema.Types.ObjectId,ref:'Cottage'},
   checkinDate: {type: Date, required: true},
   checkoutDate: {type: Date, required: true},
   adults: {type: Number},
   kids: {type: Number},
   bookStatus: [{type: String,enum:["open","closed","cancelled"]}],
 }
```

Cottage model

```javascript
{
  cottageType:{type: String, required: true, enum:["standard","classic","superior"]},
  cottageImage: {type: String},
  costPerDay: {type: String},
  about: {type: String},
  totalCottages:[{ cottageNumber: {type: Number,}, cottageStatus:{ type:String, enum:["full","free","disable"]}],
}
```

Session model

```javascript
{
  userId: [{type: Schema.Types.ObjectId,ref:'User'}],
  dateCreated: [{type: Date,}],
}
```

Membership model

```javascript
{
  userId: [{type: Schema.Types.ObjectId,ref:'User'}],
  features: [{type: String}],
  amenities: [{type: String}],
  costPerDay: [{type: String}],
}
```

<br>

## API Endpoints (backend routes)

| HTTP Method | URL                  | Request Body                                     | Success status | Error Status | Description                                                                                                                     |
| ----------- | -------------------- | ------------------------------------------------ | -------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------- |
| GET         | `/auth/validate`     | Saved session                                    | 200            | 404          | Check if user is logged in and return profile page                                                                              |
| POST        | `/auth/signup`       | {name, email, password,address,phone}            | 201            | 404          | Checks if fields not empty (422) and user not exists (409), then create user with encrypted password, and store user in session |
| POST        | `/auth/login`        | {email, password}                                | 200            | 401          | Checks if fields not empty (422), if user exists (404), and if password matches (404), then stores user in session              |
| DELETE      | `/auth/logout`       | (empty)                                          | 204            | 400          | Logs out the user                                                                                                               |
| POST        | `/user/edit/:id`     | {name, email, password,address,phone} {id}       | 200            | 400          | Edits the user profile                                                                                                          |
| GET         | `/user/all`          | {userType }                                      |                |              | Get allt customers informationtournament                                                                                        |
| DELETE      | `/user/cancel`       | {}                                               | 201            | 400          | Deletes the user                                                                                                                |
| POST        | `/booking/new/`      | {checkindate,checkoutdate,bookingdate,CottageId} | 200            | 400          | Books the Cottage on particulr days                                                                                             |
| DELETE      | `/book/cancel/:id`   | {id}                                             | 201            | 400          | delete tournament                                                                                                               |
| GET         | `/book/bookings/`    | {}                                               |                | 400          | Get all the bookings                                                                                                            |
| GET         | `/book/bookings/:id` | {id}                                             |                | 400          | Get all the bookings for a particular id                                                                                        |
| GET         | `/rooms/`            | { }                                              |                |              | Get all the rooms                                                                                                               |
| GET         | `/rooms/search`      | {searchQuery}                                    | 200            | 404          | get the rooms , based ont he query                                                                                              |
| POST        | `/rooms/new/`        | {type,image,about,cost}                          | 201            | 400          | adds a new cottage                                                                                                              |
| POST        | `/rooms/edit/:id`    | {id}                                             | 200            | 400          | edits cottage information                                                                                                       |
| GET         | `/roos/delete/:id`   | {id}                                             | 201            | 400          | delete the cottage information                                                                                                  |

+++\_
<br>

## Links

### Trello/Kanban

[Link to your trello board](https://trello.com/c/KeHbB2FR/12-create)
or picture of your physical board

### Git

The url to your repository and to your deployed project

[Client repository Link](https://github.com/SunithaMannam/resortzy-client-ui)

[Server repository Link](https://github.com/SunithaMannam/resortzy-sever)

[Deployed App Link](http://heroku.com)

### Slides

The url to your presentation slides

[Slides Link](http://slides.com)
