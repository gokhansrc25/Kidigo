# API Tasarımı - OpenAPI Specification Örneği

**OpenAPI Spesifikasyon Dosyası:** [Kidigo.yaml](Kidigo.yaml)

Bu doküman, OpenAPI Specification (OAS) 3.0 standardına göre hazırlanmış örnek bir API tasarımını içermektedir.

## OpenAPI Specification

```yaml
openapi: 3.0.3 

info: 

  title: Kidigo API 

  version: 1.0.0 

  description: | 

    Kidigo, YKS ve KPSS gibi sınavlara hazırlanan öğrenciler için geliştirilmiş  

    bir çalışma ve sınav hazırlık yönetim platformudur. 

 

  contact: 

    name: Kidigo API 

    email: api-support@kidigo.com 

  license: 

    name: MIT 

    url: https://opensource.org/licenses/MIT 

  

servers: 

  - url: https://api.kidigo.com/v1 

    description: Production Server 

  - url: https://staging-api.kidigo.com/v1 

    description: Production Server 

  - url: http://localhost:3000/v1 

    description: Development Server 

  

tags: 

  - name: auth 

    description: Kullanıcı kayıt ve giriş işlemleri  

  - name: users 

    description: Kullanıcı profil yönetimi 

  - name: study-sessions 

    description: Pomodoro ve çalışma seansları 

  - name: goals 

    description: Hedef yönetimi 

  - name: subjects 

    description: Ders yönetimi  

  - name: statistics 

    description: Performans analizleri  

  

security: 

  - bearerAuth: [] 

  

paths: 

  /auth/register: 

    post: 

      tags: [auth] 

      summary: Kullanıcı Kaydı 

      description: Yeni bir kullanıcı oluşturur ve sisteme kaydeder. 

      operationId: registerUser 

      security: [] 

      requestBody: 

        required: true 

        content: 

          application/json: 

            schema: 

              $ref: '#/components/schemas/RegisterInput' 

      responses: 

        '201': 

          description: Kullanıcı başarıyla oluşturuldu 

          content: 

            application/json: 

              schema: 

                $ref: '#/components/schemas/User' 

        '400': 

          $ref: '#/components/responses/BadRequest' 

  

  /auth/login: 

    post: 

      tags: [auth] 

      summary: Kullanıcı Girişi 

      description: Kullanıcı e-posta ve şifre ile giriş yapar. 

      operationId: loginUser 

      security: [] 

      requestBody: 

        required: true 

        content: 

          application/json: 

            schema: 

              $ref: '#/components/schemas/LoginInput' 

      responses: 

        '200': 

          description: Giriş başarılı 

          content: 

            application/json: 

              schema: 

                $ref: '#/components/schemas/AuthToken' 

        '401': 

          $ref: '#/components/responses/Unauthorized' 

  

  /users/profile: 

    get: 

      tags: [users] 

      summary: Profil Görüntüleme 

      description: Kullanıcının profil bilgilerini getirir. 

      operationId: getUserProfile 

      responses: 

        '200': 

          content: 

            application/json: 

              schema: 

                $ref: '#/components/schemas/User' 

    put: 

      tags: [users] 

      summary: Profil Güncelleme 

      description: Kullanıcı profil bilgilerini günceller. 

      operationId: updateUserProfile 

      requestBody: 

        required: true 

        content: 

          application/json: 

            schema: 

              $ref: '#/components/schemas/UserUpdateInput' 

      responses: 

        '200': 

          content: 

            application/json: 

              schema: 

                $ref: '#/components/schemas/User' 

    delete: 

      tags: [users] 

      summary: Hesap Silme (FR-5) 

      description: Kullanıcının hesabını ve tüm verilerini kalıcı olarak siler. 

      operationId: deleteUserAccount 

      responses: 

        '204': 

          description: Hesap ve veriler silindi 

  

  /study-sessions: 

    post: 

      tags: [study-sessions] 

      summary: Çalışma Seansı Başlat 

      description: Yeni bir çalışma seansı başlatır (Pomodoro veya serbest). 

      operationId: startStudySession 

      requestBody: 

        required: true 

        content: 

          application/json: 

            schema: 

              $ref: '#/components/schemas/StudySessionInput' 

      responses: 

        '201': 

          content: 

            application/json: 

              schema: 

                $ref: '#/components/schemas/StudySession' 

  

  /study-sessions/active: 

    get: 

      tags: [study-sessions] 

      summary: Aktif Seansı Görüntüle 

      description: Kullanıcının aktif olan seansını getirir. 

      operationId: getActiveSession 

      responses: 

        '200': 

          content: 

            application/json: 

              schema: 

                $ref: '#/components/schemas/StudySession' 

        '404': 

          $ref: '#/components/responses/NotFound' 

  

  /study-sessions/{sessionId}: 

    patch: 

      tags: [study-sessions] 

      summary: Seans Güncelleme 

      description: Çalışma seansının süresini veya durumunu günceller. 

      operationId: updateStudySession 

      parameters: 

        - $ref: '#/components/parameters/SessionIdParam' 

      requestBody: 

        required: true 

        content: 

          application/json: 

            schema: 

              type: object 

              properties: 

                durationMinutes: 

                  type: integer 

                  minimum: 1 

                status: 

                  type: string 

                  enum: [active, completed] 

      responses: 

        '200': 

          description: Güncellendi 

    delete: 

      tags: [study-sessions] 

      summary: Seans Silme 

      description: Belirtilen seansı siler. 

      operationId: deleteStudySession 

      parameters: 

        - $ref: '#/components/parameters/SessionIdParam' 

      responses: 

        '204': 

          description: Silindi 

  

  /study-sessions/manual: 

    post: 

      tags: [study-sessions] 

      summary: Manuel Çalışma Ekle 

      description: Kullanıcının manuel olarak eklediği çalışma süresini kaydeder. 

      operationId: createManualSession 

      requestBody: 

        required: true 

        content: 

          application/json: 

            schema: 

              $ref: '#/components/schemas/ManualStudyInput' 

      responses: 

        '201': 

          description: Kayıt oluşturuldu 

  

  /goals: 

    get: 

      tags: [goals] 

      summary: Hedefleri Listele 

      description: Kullanıcının mevcut hedeflerini getirir. 

      operationId: listGoals 

      responses: 

        '200': 

          content: 

            application/json: 

              schema: 

                type: array 

                items: 

                  $ref: '#/components/schemas/Goal' 

    post: 

      tags: [goals] 

      summary: Yeni Hedef Oluştur 

      description: Kullanıcı için yeni bir hedef oluşturur. 

      operationId: createGoal 

      requestBody: 

        required: true 

        content: 

          application/json: 

            schema: 

              $ref: '#/components/schemas/GoalInput' 

      responses: 

        '201': 

          description: Hedef oluşturuldu 

  

  /goals/{goalId}: 

    patch: 

      tags: [goals] 

      summary: Hedef Güncelleme 

      description: Mevcut hedefi günceller. 

      operationId: updateGoal 

      parameters: 

        - name: goalId 

          in: path 

          required: true 

          schema: 

            type: string 

            format: uuid 

      requestBody: 

        required: true 

        content: 

          application/json: 

            schema: 

              $ref: '#/components/schemas/GoalInput' 

      responses: 

        '200': 

          description: Güncellendi 

  

  /subjects: 

    get: 

      tags: [subjects] 

      summary: Dersleri Listele 

      description: Kullanıcının ders listesini getirir. 

      operationId: listSubjects 

      responses: 

        '200': 

          content: 

            application/json: 

              schema: 

                type: array 

                items: 

                  $ref: '#/components/schemas/Subject' 

    post: 

      tags: [subjects] 

      summary: Ders Ekle 

      description: Yeni bir ders ekler. 

      operationId: createSubject 

      requestBody: 

        required: true 

        content: 

          application/json: 

            schema: 

              $ref: '#/components/schemas/SubjectInput' 

      responses: 

        '201': 

          description: Ders eklendi 

  

  /subjects/{subjectId}: 

    put: 

      tags: [subjects] 

      summary: Ders Güncelleme 

      description: Belirtilen dersi günceller. 

      operationId: updateSubject 

      parameters: 

        - $ref: '#/components/parameters/SubjectIdParam' 

      requestBody: 

        required: true 

        content: 

          application/json: 

            schema: 

              $ref: '#/components/schemas/SubjectInput' 

      responses: 

        '200': 

          description: Güncellendi 

    delete: 

      tags: [subjects] 

      summary: Ders Silme 

      description: Belirtilen dersi siler. 

      operationId: deleteSubject 

      parameters: 

        - $ref: '#/components/parameters/SubjectIdParam' 

      responses: 

        '204': 

          description: Silindi 

  

  /statistics/summary: 

    get: 

      tags: [statistics] 

      summary: Genel Performans Özeti 

      description: Kullanıcının tüm çalışma süresi ve seans özetini getirir. 

      operationId: getStatisticsSummary 

      responses: 

        '200': 

          content: 

            application/json: 

              schema: 

                $ref: '#/components/schemas/StatisticsSummary' 

  

  /statistics/subjects: 

    get: 

      tags: [statistics] 

      summary: Ders Bazlı Analiz 

      description: Her ders için toplam çalışma süresi ve seans sayısını getirir. 

      operationId: getStatisticsBySubject 

      responses: 

        '200': 

          content: 

            application/json: 

              schema: 

                type: array 

                items: 

                  $ref: '#/components/schemas/SubjectStat' 

  

components: 

  securitySchemes: 

    bearerAuth: 

      type: http 

      scheme: bearer 

      bearerFormat: JWT 

  

  parameters: 

    SessionIdParam: 

      name: sessionId 

      in: path 

      required: true 

      schema: 

        type: string 

        format: uuid 

    SubjectIdParam: 

      name: subjectId 

      in: path 

      required: true 

      schema: 

        type: string 

        format: uuid 

  

  responses: 

    BadRequest: 

      description: Geçersiz istek 

      content: 

        application/json: 

          schema: 

            $ref: '#/components/schemas/Error' 

    Unauthorized: 

      description: Yetkisiz erişim 

      content: 

        application/json: 

          schema: 

            $ref: '#/components/schemas/Error' 

    NotFound: 

      description: Kaynak bulunamadı 

      content: 

        application/json: 

          schema: 

            $ref: '#/components/schemas/Error' 

    InternalServerError: 

      description: Sunucu hatası 

      content: 

        application/json: 

          schema: 

            $ref: '#/components/schemas/Error' 

  

  schemas: 

    User: 

      type: object 

      required: [id, email, username] 

      properties: 

        id: 

          type: string 

          format: uuid 

          example: "f47ac10b-58cc-4372-a567-0e02b2c3d479" 

        username: 

          type: string 

          example: "gokhan.saricicek" 

        email: 

          type: string 

          format: email 

          example: "gokhan@gmail.com" 

        targetExam: 

          type: string 

          example: "YKS" 

        createdAt: 

          type: string 

          format: date-time 

          example: "2026-03-13T16:30:00Z" 

  

    AuthToken: 

      type: object 

      required: [token, user] 

      properties: 

        token: 

          type: string 

          example: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." 

        expiresIn: 

          type: integer 

          example: 3600 

        user: 

          $ref: '#/components/schemas/User' 

  

    RegisterInput: 

      type: object 

      required: [username, email, password] 

      properties: 

        username: 

          type: string 

          minLength: 3 

          example: "gokhan.saricicek" 

        email: 

          type: string 

          format: email 

          example: " gokhan@gmail.com " 

        password: 

          type: string 

          minLength: 8 

          example: "Sifre1234!" 

        targetExam: 

          type: string 

          example: "YKS" 

  

    LoginInput: 

      type: object 

      required: [email, password] 

      properties: 

        email: 

          type: string 

          format: email 

          example: " gokhan@gmail.com " 

        password: 

          type: string 

          example: "Sifre1234!" 

  

    UserUpdateInput: 

      type: object 

      properties: 

        username: 

          type: string 

          example: "gokhan.saricicek" 

        targetExam: 

          type: string 

          example: "KPSS" 

  

    StudySession: 

      type: object 

      properties: 

        id: 

          type: string 

          format: uuid 

          example: "9f8c7d6b-1234-5678-9abc-0def12345678" 

        subjectId: 

          type: string 

          format: uuid 

          example: "c3d5e7f8-1234-5678-9abc-0def98765432" 

        durationMinutes: 

          type: integer 

          minimum: 0 

          example: 25 

        startTime: 

          type: string 

          format: date-time 

          example: "2026-03-13T16:30:00Z" 

        endTime: 

          type: string 

          format: date-time 

          example: "2026-03-13T16:55:00Z" 

        status: 

          type: string 

          enum: [active, completed] 

          example: "active" 

        type: 

          type: string 

          enum: [pomodoro, free] 

          example: "pomodoro" 

  

    StudySessionInput: 

      type: object 

      required: [subjectId] 

      properties: 

        subjectId: 

          type: string 

          format: uuid 

          example: "c3d5e7f8-1234-5678-9abc-0def98765432" 

  

    ManualStudyInput: 

      type: object 

      required: [subjectId, durationMinutes, date] 

      properties: 

        subjectId: 

          type: string 

          format: uuid 

          example: "c3d5e7f8-1234-5678-9abc-0def98765432" 

        durationMinutes: 

          type: integer 

          minimum: 1 

          example: 45 

        date: 

          type: string 

          format: date 

          example: "2026-03-13" 

  

    Goal: 

      type: object 

      properties: 

        id: 

          type: string 

          format: uuid 

          example: "a1b2c3d4-5678-90ab-cdef-1234567890ab" 

        title: 

          type: string 

          example: "Matematik Çalışması" 

        targetMinutes: 

          type: integer 

          example: 60 

        period: 

          type: string 

          enum: [daily, weekly] 

          example: "daily" 

        completionPercent: 

          type: integer 

          example: 50 

  

    GoalInput: 

      type: object 

      required: [title, targetMinutes, period] 

      properties: 

        title: 

          type: string 

          example: "Matematik Çalışması" 

        targetMinutes: 

          type: integer 

          minimum: 1 

          example: 60 

        period: 

          type: string 

          enum: [daily, weekly] 

          example: "daily" 

  

    Subject: 

      type: object 

      properties: 

        id: 

          type: string 

          format: uuid 

          example: "c3d5e7f8-1234-5678-9abc-0def98765432" 

        name: 

          type: string 

          example: "Matematik" 

  

    SubjectInput: 

      type: object 

      required: [name] 

      properties: 

        name: 

          type: string 

          minLength: 2 

          example: "Matematik" 

  

    SubjectStat: 

      type: object 

      properties: 

        subjectName: 

          type: string 

          example: "Matematik" 

        totalMinutes: 

          type: integer 

          example: 120 

        sessionCount: 

          type: integer 

          example: 3 

  

    StatisticsSummary: 

      type: object 

      properties: 

        totalStudyTime: 

          type: integer 

          example: 360 

        completedSessions: 

          type: integer 

          example: 10 

        dailyAverage: 

          type: integer 

          example: 45 

  

    Error: 

      type: object 

      required: [code, message] 

      properties: 

        code: 

          type: string 

          example: "VALIDATION_ERROR" 

        message: 

          type: string 

          example: "Geçersiz veri girişi." 

        details: 

          type: array 

          items: 

            type: object 
```
