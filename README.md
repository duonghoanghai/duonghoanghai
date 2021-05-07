- 👋 Hi, I’m @duonghoanghai
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
duonghoanghai/duonghoanghai is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
// copy hàng của tao là tao coppy lại hàng chính phủ tối cao của các ngươi và ném sót rác đó;
// inc với github and pio ; tên doanh nghiệp là aries cubecell pio github;
// mã số doanh nghiệp pio đăng ký;
// mã số thuế cũng vậy;
// yêu cầu cập nhật danh sách thành viên ;
version: 2
jobs:
  build:
    working_directory: ~/code
    docker:
      - image: circleci/android:api-30-alpha
        auth:
          username: mydockerhub-user
          password: $DOCKERHUB_PASSWORD  # context / project UI env-var reference
    environment:
      JVM_OPTS: -Xmx3200m
    steps:
      - checkout
      - restore_cache:
          key: jars-{{ checksum "build.gradle" }}-{{ checksum  "app/build.gradle" }}
#      - run:
#         name: Chmod permissions #if permission for Gradlew Dependencies fail, use this.
#         command: sudo chmod +x ./gradlew
      - run:
          name: Download Dependencies
          command: ./gradlew androidDependencies
      - save_cache:
          paths:
            - ~/.gradle
          key: jars-{{ checksum "build.gradle" }}-{{ checksum  "app/build.gradle" }}
      - run:
          name: Run Tests
          command: ./gradlew lint test
      - store_artifacts: # for display in Artifacts: https://circleci.com/docs/2.0/artifacts/
          path: app/build/reports
          destination: reports
      - store_test_results: # for display in Test Summary: https://circleci.com/docs/2.0/collect-test-data/
          path: app/build/test-results
      # See https://circleci.com/docs/2.0/deployment-integrations/ for deploy examples
