<script>
import Footer from './footer.vue'

import { initializeApp } from "firebase/app";
import { getFirestore, collection, addDoc } from "firebase/firestore"

export default {
    props: ["appYear", "currentPage"],
    data() {
        return {
            firebase: {
                config: {
                    apiKey: "apikey",
                    authDomain: "domain",
                    projectId: "id",
                    storageBucket: "bucket",
                    messagingSenderId: "senderid",
                    appId: "appid"
                },
                app: null,
                db: null,
                docRef: null,
                docSnap: null
            }
        }
    },
    components: {
        Footer
    },
    methods: {
        async sendResponse() {
            this.firebase.app = initializeApp(this.firebase.config);
            this.firebase.db = getFirestore(this.firebase.app);

            let name = $('#name').val();
            let comment = $('#comment').val();

            try {
                this.firebase.docRef = await addDoc(collection(this.firebase.db, "Form_Submissions"), {
                    NAME: name,
                    COMMENT: comment
                });
                $("#sent_message").html("Successfully Submitted. Thank you...");
            }
            catch(e) {
                // form not submitted
                console.log(e)
                $("#sent_message").html("Submission Failed. Please try again later...");
            }

        }
    },
    computed: {
        computePage() {
            return this.currentPage;
        }
    },
    watch: {
        computePage(page) {
            if (page === "form") {
                $(".form_img").attr("src", $(".form_img").attr("data"));
                $("#sent_message").html("");
                $("#name").val("");
                $("#comment").val("");
            }
        }
    }
}

</script>
<template>
    <div class="main-container">
        <img class="background_image form_img" src="" data="../../images/Gooseberry Falls-min.jpg" alt="Gooseberry Falls">
        <div class="form-container">
            <div class="form-div">
                <div class="form">
                    <div class="grid-x">
                        <div class="large-2 medium-2 small-2 cell"></div>
                        <div class="large-8 medium-8 small-8 cell">
                            <h1 class="form-header">Form</h1>
                            <p>If there is something you wish to contact us about you
                                may do so here. If you wish to stay anynomous
                                please type 'anynomous' in the name field.
                            </p>
                            <label class="form-label" for="name-text">Name:</label>
                            <input class="form-input" placeholder="Paul Bunyan" type="text" name="name-text" id="name">
                            <label class="form-label" for="comment" >Comment:</label>
                            <textarea name="comment" cols="20" rows="2" id="comment" placeholder="Comment here..."></textarea>
                            <button @click="sendResponse" type="submit" class="form-btn button">Submit</button>
                            <div id="sent_message" class="submission_text"></div>
                        </div>
                        <div class="large-2 medium-2 small-2 cell"></div>
                    </div>

                </div>
            </div>
        </div>
    </div>
    <Footer :year="this.appYear"></Footer>
</template>
<style>
.main-container {
    height: fit-content;
}

.background_image {
    object-fit: cover;
    height: 700px;
    width: 100%;
    background-color: white;
    opacity: 0.5;
}

.form-container {
    position: absolute;
    top: 0;
    width: 100%;
    height: fit-content;
}

.form-div {
    display: flex;
    justify-content: center;
    align-items: center;
    height: fit-content;
}

.form {
    height: fit-content;
    width: 50%;
    background-color: rgba(255, 247, 230, 0.8);
    border-radius: 20px;
    color: black;
    margin-top: 100px; 
    margin-bottom: 100px; 
}

.form-header {
    text-align: center;
}

.form-label {
    font-size: 1.2rem;
    color: black;
}

.form-btn {
    width: 100%;
    background-color: var(--main_color_1);
    margin-bottom: 30px;
}

.form-btn:hover {
    background-color: var(--main_color_2);
}

.submission_text {
    color: red; 
    font-size: 1.2rem;
    text-align: center; 
}
</style>
