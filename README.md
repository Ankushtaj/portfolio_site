erDiagram

    USERS {
        int user_id PK
        varchar email
        varchar password_hash
        varchar first_name
        varchar last_name
        varchar phone
        enum role
        boolean is_active
        datetime created_at
    }

    WEBINAR {
        int webinar_id PK
        varchar title
        datetime start_datetime
        datetime end_datetime
        varchar platform
        enum w_type
        enum w_status
        int max_capacity
        varchar recording_url
        int organizer_id FK
        datetime created_at
    }

    SESSION {
        int session_id PK
        varchar session_title
        enum session_type
        datetime start_datetime
        datetime end_datetime
        int session_order
        int webinar_id FK
    }

    SESSION_SPEAKER {
        int session_id FK
        int speaker_id FK
        enum role_in_session
        decimal honorarium
    }

    REGISTRATION {
        int reg_id PK
        int attendee_id FK
        int webinar_id FK
        datetime reg_datetime
        enum reg_status
        enum attendance_status
        boolean certificate_issued
    }

    CERTIFICATE {
        int cert_id PK
        int reg_id FK
        varchar cert_url
        varchar cert_code
        datetime issued_at
    }

    FEEDBACK {
        int feedback_id PK
        int attendee_id FK
        int webinar_id FK
        int session_id FK
        int overall_rating
        text comments
        datetime submitted_at
    }

    TAG {
        int tag_id PK
        varchar tag_name
    }

    WEBINAR_TAG {
        int webinar_id FK
        int tag_id FK
    }

    WAITLIST {
        int waitlist_id PK
        int user_id FK
        int webinar_id FK
        datetime joined_at
        enum status
    }

    USERS ||--o{ WEBINAR : organizes
    USERS ||--o{ REGISTRATION : registers
    USERS ||--o{ FEEDBACK : gives
    USERS ||--o{ WAITLIST : joins
    USERS ||--o{ SESSION_SPEAKER : speaks_in

    WEBINAR ||--o{ SESSION : has
    WEBINAR ||--o{ REGISTRATION : receives
    WEBINAR ||--o{ FEEDBACK : gets
    WEBINAR ||--o{ WAITLIST : has
    WEBINAR ||--o{ WEBINAR_TAG : tagged_with

    SESSION ||--o{ SESSION_SPEAKER : has_speakers
    SESSION ||--o{ FEEDBACK : gets

    TAG ||--o{ WEBINAR_TAG : used_in

    REGISTRATION ||--|| CERTIFICATE : earns
