##assign - использовать для объявление собственной переменной и присвоения значения

##capture - сохраняет результат выполнения шаблона (обычно текст или HTML) в переменную. Главная идея capture — не "перезапись", а возможность сначала собрать HTML, а потом вывести его в другом месте.

##if - логическая проверка

##unless - выполнения чего либо пока соблюдается условие. То есть это просто отрицательная форма if (не цикл).

##for - цикл перебора значений

##render - Подключает другой шаблон (обычно snippet) в изолированной области видимости (scope) и передает ему параметры. Именно изоляция — главная причина появления render.

##filters - функция фильтра которая может использоваться в циклах, выводах значений и тд.


##Filters examples:

<div class="field">
  <input
    class="field__input"
    name="{{ filter.min_value.param_name }}"
    id="{{ id_prefix }}{{ filter.label | escape }}-GTE"
    {%- if filter.min_value.value -%}
      value="{{ filter.min_value.value | money_without_currency }}"
    {%- endif -%}
    type="text"
    inputmode="decimal"
    placeholder="0"
    data-pattern="\d| |,|\."
    data-min="0"
    data-max="{{ filter.range_max | money_without_currency }}"
  >
  <label class="field__label" for="{{ id_prefix }}{{ filter.label | escape }}-GTE">
    {{- 'products.facets.from' | t -}}
  </label>
</div>

{%- if filter_type != 'vertical' -%}
  <span class="field-currency">{{ cart.currency.symbol }}</span>
{%- endif -%}

<div class="field">
  <input
    class="field__input"
    name="{{ filter.max_value.param_name }}"
    id="{{ id_prefix }}{{ filter.label | escape }}-LTE"
    {%- if filter.max_value.value -%}
      value="{{ filter.max_value.value | money_without_currency }}"
    {%- endif -%}
    type="text"
    inputmode="decimal"
    placeholder="{{ filter.range_max | money_without_currency }}"
    data-pattern="\d| |,|\."
    data-min="0"
    data-max="{{ filter.range_max | money_without_currency }}"
  >
  <label class="field__label" for="{{ id_prefix }}{{ filter.label | escape }}-LTE">
    {{- 'products.facets.to' | t -}}
  </label>
</div>


##Objects examples: 

<div class="rating-wrapper">
                      {% liquid
                        assign rating_decimal = 0
                        assign decimal = product.metafields.reviews.rating.value.rating | modulo: 1
                        if decimal >= 0.3 and decimal <= 0.7
                          assign rating_decimal = 0.5
                        elsif decimal > 0.7
                          assign rating_decimal = 1
                        endif
                      %}
                      <div
                        class="rating"
                        role="img"
                        aria-label="{{ 'accessibility.star_reviews_info' | t: rating_value: product.metafields.reviews.rating.value, rating_max: product.metafields.reviews.rating.value.scale_max }}"
                      >
                        <span
                          aria-hidden="true"
                          class="rating-star"
                          style="--rating: {{ product.metafields.reviews.rating.value.rating | floor }}; --rating-max: {{ product.metafields.reviews.rating.value.scale_max }}; --rating-decimal: {{ rating_decimal }};"
                        ></span>
                      </div>
                      <p class="rating-text caption">
                        <span aria-hidden="true">
                          {{- product.metafields.reviews.rating.value }} /
                          {{ product.metafields.reviews.rating.value.scale_max -}}
                        </span>
                      </p>
                      <p class="rating-count caption">
                        <span aria-hidden="true">({{ product.metafields.reviews.rating_count }})</span>
                        <span class="visually-hidden">
                          {{- product.metafields.reviews.rating_count }}
                          {{ 'accessibility.total_reviews' | t -}}
                        </span>
                      </p>
                    </div>



##Tags examples: 

<div class="color-{{ section.settings.color_scheme }} gradient">
  <div class="contact page-width page-width--narrow section-{{ section.id }}-padding">
    {%- if section.settings.heading != blank -%}
      <h2 class="title title-wrapper--no-top-margin inline-richtext {{ section.settings.heading_size }}{% if settings.animations_reveal_on_scroll %} scroll-trigger animate--slide-in{% endif %}">
        {{ section.settings.heading }}
      </h2>
    {%- else -%}
      <h2 class="visually-hidden">{{ 'templates.contact.form.title' | t }}</h2>
    {%- endif -%}
    {%- liquid
      assign contact_form_class = 'isolate'
      if settings.animations_reveal_on_scroll
        assign contact_form_class = 'isolate scroll-trigger animate--slide-in'
      endif
    -%}
    {%- form 'contact', id: 'ContactForm', class: contact_form_class -%}
      {%- if form.posted_successfully? -%}
        <h2 class="form-status form-status-list form__message" tabindex="-1" autofocus>
          {{- 'icon-success.svg' | inline_asset_content -}}
          {{ 'templates.contact.form.post_success' | t }}
        </h2>
      {%- elsif form.errors -%}
        <div class="form__message">
          <h2 class="form-status caption-large text-body" role="alert" tabindex="-1" autofocus>
            {{- 'icon-error.svg' | inline_asset_content -}}
            {{ 'templates.contact.form.error_heading' | t }}
          </h2>
        </div>
        <ul class="form-status-list caption-large" role="list">
          <li>
            <a href="#ContactForm-email" class="link">
              {{ form.errors.translated_fields.email | capitalize }}
              {{ form.errors.messages.email }}
            </a>
          </li>
        </ul>
      {%- endif -%}
      <div class="contact__fields">
        <div class="field">
          <input
            class="field__input"
            autocomplete="name"
            type="text"
            id="ContactForm-name"
            name="contact[{{ 'templates.contact.form.name' | t }}]"
            value="{% if form.name %}{{ form.name }}{% elsif customer %}{{ customer.name }}{% endif %}"
            placeholder="{{ 'templates.contact.form.name' | t }}"
          >
          <label class="field__label" for="ContactForm-name">{{ 'templates.contact.form.name' | t }}</label>
        </div>
        <div class="field field--with-error">
          <input
            autocomplete="email"
            type="email"
            id="ContactForm-email"
            class="field__input"
            name="contact[email]"
            spellcheck="false"
            autocapitalize="off"
            value="{% if form.email %}{{ form.email }}{% elsif customer %}{{ customer.email }}{% endif %}"
            aria-required="true"
            {% if form.errors contains 'email' %}
              aria-invalid="true"
              aria-describedby="ContactForm-email-error"
            {% endif %}
            placeholder="{{ 'templates.contact.form.email' | t }}"
          >
          <label class="field__label" for="ContactForm-email">
            {{- 'templates.contact.form.email' | t }}
            <span aria-hidden="true">*</span></label
          >
          {%- if form.errors contains 'email' -%}
            <small class="contact__field-error" id="ContactForm-email-error">
              <span class="visually-hidden">{{ 'accessibility.error' | t }}</span>
              <span class="form__message">
                <span class="svg-wrapper">
                  {{- 'icon-error.svg' | inline_asset_content -}}
                </span>
                {{- form.errors.translated_fields.email | capitalize }}
                {{ form.errors.messages.email -}}
              </span>
            </small>
          {%- endif -%}
        </div>
      </div>
      <div class="field">
        <input
          type="tel"
          id="ContactForm-phone"
          class="field__input"
          autocomplete="tel"
          name="contact[{{ 'templates.contact.form.phone' | t }}]"
          pattern="[0-9\-]*"
          value="{% if form.phone %}{{ form.phone }}{% elsif customer %}{{ customer.phone }}{% endif %}"
          placeholder="{{ 'templates.contact.form.phone' | t }}"
        >
        <label class="field__label" for="ContactForm-phone">{{ 'templates.contact.form.phone' | t }}</label>
      </div>
      <div class="field">
        <textarea
          rows="10"
          id="ContactForm-body"
          class="text-area field__input"
          name="contact[{{ 'templates.contact.form.comment' | t }}]"
          placeholder="{{ 'templates.contact.form.comment' | t }}"
        >
          {{- form.body -}}
        </textarea>
        <label class="form__label field__label" for="ContactForm-body">
          {{- 'templates.contact.form.comment' | t -}}
        </label>
      </div>
      <div class="contact__button">
        <button type="submit" class="button">
          {{ 'templates.contact.form.send' | t }}
        </button>
      </div>
    {%- endform -%}
  </div>
</div>