<template>
    <section class="section-contact-form">

        <form>
            <p>{{item.fields.formIntroText}}</p>
            
            <div class="form-group">
                <label for="fieldName">{{item.fields.formLabelName}}</label>
                <input type="text" id="fieldName" />
            </div>

            <div class="form-group">
                <label for="fieldEmail">{{item.fields.formLabelEmail}}</label>
                <input type="email" id="fieldEmail" />
            </div>

            <div class="form-group">
                <label for="fieldPhone">{{item.fields.formLabelPhone}}</label>
                <input type="phone" id="fieldPhone" />
            </div>

            <div class="form-group">
                <label for="fieldCompany">{{item.fields.formLabelCompany}}</label>
                <input type="text" id="fieldCompany" />
            </div>

            <div class="form-group">
                <label for="fieldComments">{{item.fields.formLabelComments}}</label>
                <textarea rows="" cols="" id="fieldComments"></textarea>
            </div>

            <button type="submit" class="btn btn-md">{{item.fields.formButtonLabel}}</button>
        </form>

    </section>
</template>

<script>
export default {
  data: function() {
    return {
      
    }
  },
  props: {
    contentID: Number,
    item: Object,
    page: Object,
    pageInSitemap: Object,
    dynamicPageItem: Object
  }
};
</script>
