<template>
  <div class="about mt-42">
    <h1>About Us</h1>
    <p>{{ companyInfo.description }}</p>

    <h2>Our Team</h2>
    <div class="team">
      <div v-for="member in teamMembers" :key="member.id" class="team-member">
        <h3>{{ member.name.firstname }} {{ member.name.lastname }}</h3>
        <p>
          Email:
          <a :href="'mailto:' + member.email">{{ member.email }}</a>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "AboutComponent",
  data() {
    return {
      companyInfo: {
        name: "Demo Company",
        description:
          "We are a leading company in the tech industry, focused on innovation and customer satisfaction.",
      },
      teamMembers: [],
    };
  },
  mounted() {
    axios
      .get("https://fakestoreapi.com/users")
      .then((response) => {
        this.teamMembers = response.data;
      })
      .catch((error) => console.error("Error fetching team members:", error));
  },
};
</script>

<style lang="scss" scoped>
.about {
  padding: 30px;
  background-color: $bg-main-color;
  border-radius: 12px;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s ease;

  h1 {
    color: $color-orange;
    font-size: 2.2rem;
    font-weight: 700;
    margin-bottom: 0.8em;
  }

  h2 {
    color: $color-lightpurple;
    font-size: 1.8rem;
    font-weight: 600;
    margin-top: 2em;
    margin-bottom: 0.7em;
  }

  p {
    font-size: 1.1rem;
    color: $color-white;
    line-height: 1.8;
  }

  .team {
    display: flex;
    flex-direction: column;
    gap: 1.5em;

    .team-member {
      background-color: $color-darkblue;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
      transition: transform 0.3s ease, box-shadow 0.3s ease;

      &:hover {
        transform: translateY(-6px);
        box-shadow: 0 8px 16px rgba(0, 0, 0, 0.15);
      }

      h3 {
        color: $color-lightblue;
        font-size: 1.4rem;
        margin-bottom: 5px;
        font-weight: 600;
      }

      p {
        font-size: 1rem;
        color: $color-muted;

        a {
          color: $color-lightgreen;
          text-decoration: none;
          font-weight: 500;

          &:hover {
            text-decoration: underline;
            color: $color-lightpink;
          }
        }
      }
    }
  }
}
</style>
